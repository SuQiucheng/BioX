<template>
  <div class="header">
    <div>
      BraInward BioX
      <a-button @click="openAtDebug">
        <template #icon>
          <BugOutlined />
        </template>
      </a-button>
    </div>
    <a-popover
      v-if="!isConnect"
      overlayClassName="header-device-popover"
      v-model:open="connectVisible"
      :overlayStyle="{ width: '300px' }"
      trigger="click"
    >
      <template #content>
        <div>
          <ul class="header-device-list" v-if="deviceList.length">
            <li
              v-for="item in deviceList"
              :key="item.deviceId"
              class="header-device-item"
              @click="connectDevice(item)"
            >
              <DeploymentUnitOutlined />
              <div>
                <p>{{ item.deviceName }}</p>
                <p>{{ item.deviceId }}</p>
              </div>
            </li>
          </ul>
          <div v-else class="header-device-loading">
            <a-spin />
          </div>
        </div>
      </template>
      <div class="header-headset" @click="openConnectVisible">
        <span>Connect device</span>
      </div>
    </a-popover>

    <div v-else>
      <a-dropdown placement="bottom" trigger="click" arrow>
        <div class="header-center">
          <span>BIO X</span>
          <div class="electricity">
            <span>100</span>
          </div>
        </div>
        <template #overlay>
          <a-menu>
            <a-menu-item>
              <div @click="closeDevice" style="text-align: center; color: #666">
                断开连接
              </div>
            </a-menu-item>
          </a-menu>
        </template>
      </a-dropdown>
    </div>
    <div>
      <a-space>
        <a-button danger type="primary" @click="closeWindow">
          <template #icon>
            <CloseCircleOutlined />
          </template>
        </a-button>
      </a-space>
    </div>
  </div>
  <a-modal
    v-model:open="openATModal"
    title="AT指令调试"
    cancelText="取消"
    width="40%"
    :maskClosable="false"
    okText="发送"
    @ok="handleStartATModal"
    @cancel="handleEndATModal"
  >
    <span style="color: #999">例：AT+STOP_ALL(无需回车)</span>
    <a-textarea
      style="margin-top: 5px"
      v-model:value="ATValue"
      placeholder=""
      :rows="6"
    />
    <br />
    <br />
    <div class="at-content" ref="atContent">
      <p v-for="(item, index) in atNoticeList" :key="index">
        <span style="margin-right: 5px">{{
          formatTimestamp(item.time, "yyyy-MM-dd HH:mm:ss", true)
        }}</span>
        <span style="margin-right: 5px">{{
          item.type === 1 ? "发:" : "收:"
        }}</span>
        <span>{{ item.content }}</span>
      </p>
    </div>
  </a-modal>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted, getCurrentInstance, watch ,nextTick} from "vue";
const ipcRenderer = require("electron").ipcRenderer;
import { DeploymentUnitOutlined } from "@ant-design/icons-vue";
import { CustomBluetooth } from "../utils/bluetooth";
import { CustomDatabase } from "../utils/db";
import { formatTimestamp } from "../utils/common";
import { CloseCircleOutlined, BugOutlined } from "@ant-design/icons-vue";
import { useIndexStore } from "../store/index";
import { storeToRefs } from "pinia";
const indexStore = useIndexStore();
const { isConnect } = storeToRefs(indexStore);
import { createVNode } from "vue";
const connectVisible = ref<boolean>(false);
const ATValue = ref("");
import { message } from "ant-design-vue";
const bluetooth = new CustomBluetooth();
const app = getCurrentInstance();
const db = new CustomDatabase();
const openATModal = ref(false);
import { Modal } from "ant-design-vue";
let timer_uuid: any = null;
const atContent: any = ref(null);

interface DeviceItem {
  deviceId: string;
  deviceName: string;
}
interface NoticeItem {
  time: number;
  type: number;
  content: string;
}
let selectDeviceItem: DeviceItem | null = null;
const deviceList = ref<DeviceItem[]>([]);
const atNoticeList = reactive<NoticeItem[]>([]);

const openConnectVisible = () => {
  connectVisible.value = true;
  bluetooth.scan();
};

// watch(connectVisible, (value) => {
//   if (!value && !selectDeviceItem) {
//     // 取消蓝牙扫描
//     // ipcRenderer.send("cancel-bluetooth-request");
//   } else {
//     selectDeviceItem = null;
//     debugger;
//   }
// });

// 蓝牙扫描和连接
const findDevice = () => {
  bluetooth.init((value, msg) => {
    if (value) {
      switch (msg) {
        case "initComplete":
          connectBluetooth();
          break;
        case "loading":
          app?.proxy?.loading.show("连接设备中...");
          break;
        case "hide":
          app?.proxy?.loading.hide();
          break;
        case "success":
          selectDeviceItem = null;
          isConnect.value = true;
          message.success("连接成功");
      }
    } else {
      app?.proxy?.loading.hide();
      if (msg !== "User cancelled the requestDevice() chooser.") {
        message.error(msg);
      }
    }
  }, selectDeviceItem?.deviceId);
};

// 关闭应用
const closeWindow = () => {
  ipcRenderer.send("close-window");
};

// 确认连接设备
const connectDevice = (item) => {
  app?.proxy?.loading.show();
  selectDeviceItem = item;
  // 取消蓝牙扫描
  ipcRenderer.send("cancel-bluetooth-request");
  console.log("selectDeviceItem", selectDeviceItem);
  db.all(`select * from device`).then((list) => {
    const device = list.find((i) => i.deviceId === item.deviceId);
    if (device) {
      // 连接过的设备，直接连接
      findDevice();
    } else {
      findDevice();
      // app?.proxy?.loading.show("获取UUID中...");
      // timer_uuid = setTimeout(() => {
      //   app?.proxy?.loading.hide();
      //   timer_uuid && clearTimeout(timer_uuid);
      //   return message.error("获取UUID失败");
      // }, 120 * 1000);
      // // 未连接过的设备，调python生成uuid
      // ipcRenderer.send("python-uuid", item.deviceId);
    }
  });

  connectVisible.value = false;
};

const connectBluetooth = () => {
  // 发起连接
  ipcRenderer.send(
    "connect-bluetooth-device",
    JSON.stringify(selectDeviceItem)
  );
};

// 蓝牙断开
const closeDevice = () => {
  selectDeviceItem = null;
  bluetooth.close((value, msg) => {
    isConnect.value = false;
    message.success(msg);
  });
};

// 测试蓝牙扫描
const findDeviceList = () => {
  bluetooth.bluetoothScan();
};
// 用户点击，开始连接蓝牙
const clickme = () => {
  findDevice();
};

// at通知事件
const atNotice = (value: string) => {
  console.log("at通知事件", value);
  atNoticeList.push({
    time: new Date().getTime(),
    type: 2,
    content: value,
  });
  scrollToBottom()
};

// 打开AT调试
const openAtDebug = () => {
  ATValue.value = "";
  bluetooth.addATNotice(atNotice);
  openATModal.value = true;
  scrollToBottom()
};

// 关闭AT调试
const handleEndATModal = () => {
  bluetooth.removeATNotice(atNotice);
  openATModal.value = false;
};

// 发送AT指令
const handleStartATModal = () => {
  atNoticeList.push({
    time: new Date().getTime(),
    type: 1,
    content: ATValue.value,
  });
  bluetooth.sendAT(ATValue.value);
  scrollToBottom()
};

const scrollToBottom = () => {
  nextTick(()=> {
    if (atContent.value) {
    atContent.value.scrollTo({ behavior: "smooth", top: atContent.value.scrollHeight });
  }
  })

};

onMounted(() => {
  // 蓝牙扫描出来的设备
  ipcRenderer.on("find-device", (event, data) => {
    deviceList.value = data;
  });
  // python生成uuid
  ipcRenderer.on("python-uuid-response", (event, data) => {
    timer_uuid && clearTimeout(timer_uuid);
    data = JSON.parse(data);
    if (!data.address) {
      app?.proxy?.loading.hide();
      return message.error("获取UUID失败");
    }
    db.insert("device", {
      deviceId: data.address,
      uuidList: JSON.stringify(data.services),
      name: data.name,
      describe: "",
      createTime: new Date().getTime(),
    })
      .then((res) => {
        // 获取uuid后，直接调用会报错，所以得弹出框对话框
        Modal.confirm({
          title: "确认要连接设备",
          content: createVNode("div", null, [
            createVNode("p", null, `Name：${data.name}`),
            createVNode(
              "p",
              null,
              `UUID：${data.services[data.services.length - 1].uuid}`
            ),
          ]),
          okText: "确认",
          cancelText: "取消",
          onOk() {
            findDevice();
          },
          onCancel() {
            app?.proxy?.loading.hide();
          },
        });
      })
      .catch((err) => {
        console.log("error", err);
      });
  });
  //
  ipcRenderer.on("select-bluetooth-callback", (event, data) => {
    if (!data) {
      // 执行失败
      app?.proxy?.loading.hide();
      return message.error(
        "执行失败，请不用频繁操作，并确保蓝牙设备处于广播状态！"
      );
    }
  });
});
</script>
<style scoped></style>
