<template>
  <div class="charts-wrap fnirs-wrap">
    <p class="eig-nav-title">FNIRS</p>
    <div class="eig-card card-top">
      <p class="card-title">Time Series</p>
      <div
        style="width: 100%; height: 100%"
        class="time-series"
        id="series"
      ></div>
    </div>
    <div class="card-bottom">
      <div class="eig-card">
        <p class="card-title">Channel Selection</p>
        <div class="selection-box-wrap">
          <div class="selection" id="selection">
            <div class="selection-box">
              <div
                class="S1-D5 round-line"
                :style="`width: calc((${selectionHeight / 2}px + 50%) / 2);`"
              >
                <div class="S1 round"></div>
                <div
                  class="S1-D5 line-box"
                  @click="channelLineClick(1)"
                  :class="{ active: channels.includes(1) }"
                >
                  <span class="line"></span>
                </div>
                <div class="D5 round" @click="channelLineClick(1)">
                  <span class="D5-text text">D5</span>
                </div>
              </div>
              <div
                class="S1-D7 round-line"
                :style="`width: calc((${selectionHeight / 2}px + 50%) / 2);`"
              >
                <div
                  class="S1 round"
                  @click="channelLineClick([1, 2, 3, 4])"
                ></div>
                <span class="S1-text text">S1</span>
                <div
                  class="S1-D7 line-box"
                  @click="channelLineClick(3)"
                  :class="{ active: channels.includes(3) }"
                >
                  <span class="line"></span>
                </div>
                <div class="D7 round" @click="channelLineClick(3)">
                  <span class="D7-text text">D7</span>
                </div>
              </div>
              <div class="D8 round" @click="channelLineClick(4)">
                <span class="D8-text text">D8</span>
              </div>
              <div class="D6 round" @click="channelLineClick(2)">
                <span class="D6-text text">D6</span>
              </div>
              <div
                class="S1-D8 line-box"
                :class="{ active: channels.includes(4) }"
              >
                <svg
                  class="S1-D8-SVG"
                  width="100%"
                  @click="channelLineClick(4)"
                  height="25"
                  viewBox="0 0 220 25"
                  preserveAspectRatio="none"
                >
                  <path
                    d="M0,25 Q110,-20, 220,25"
                    :stroke="channels.includes(4) ? '#00D008' : '#848484'"
                    fill="transparent"
                    stroke-width="3"
                  />
                </svg>
                <!-- <span class="line"></span> -->
              </div>
              <div class="S1-D6 line-box">
                <svg
                  class="S1-D6-SVG"
                  width="100%"
                  @click="channelLineClick(2)"
                  height="16"
                  viewBox="0 0 220 16"
                  preserveAspectRatio="none"
                >
                  <path
                    d="M0,0 Q110,16, 220,0"
                    :stroke="channels.includes(2) ? '#00D008' : '#848484'"
                    fill="transparent"
                    stroke-width="3"
                  />
                </svg>
              </div>
            </div>
            <div class="selection-box selection-box-right">
              <div
                class="S1-D5 round-line"
                :style="`width: calc((${selectionHeight / 2}px + 50%) / 2);`"
              >
                <div class="D5 round" @click="channelLineClick(5)">
                  <span class="D5-text text">D1</span>
                </div>
                <div
                  @click="channelLineClick(5)"
                  :class="{ active: channels.includes(5) }"
                  class="S1-D5 line-box"
                >
                  <span class="line"></span>
                </div>
                <div class="S1 round"></div>
              </div>
              <div
                class="S1-D7 round-line"
                :style="`width: calc((${selectionHeight / 2}px + 50%) / 2);`"
              >
                <div class="D7 round" @click="channelLineClick(7)">
                  <span class="D7-text text">D3</span>
                </div>
                <div
                  class="S1-D7 line-box"
                  @click="channelLineClick(7)"
                  :class="{ active: channels.includes(7) }"
                >
                  <span class="line"></span>
                </div>
                <div class="S1 round" @click="channelLineClick([5, 6, 7, 8])">
                  <span class="S1-text text">S2</span>
                </div>
              </div>
              <div class="D8 round" @click="channelLineClick(8)">
                <span class="D8-text text">D4</span>
              </div>
              <div class="D6 round" @click="channelLineClick(6)">
                <span class="D6-text text">D2</span>
              </div>
              <div
                class="S1-D8 line-box"
                :class="{ active: channels.includes(8) }"
              >
                <svg
                  class="S1-D8-SVG"
                  width="100%"
                  @click="channelLineClick(8)"
                  height="25"
                  viewBox="0 0 220 25"
                  preserveAspectRatio="none"
                >
                  <path
                    d="M0,25 Q110,-20, 220,25"
                    :stroke="channels.includes(8) ? '#00D008' : '#848484'"
                    fill="transparent"
                    stroke-width="3"
                  />
                </svg>
                <!-- <span class="line"></span> -->
              </div>
              <div class="S1-D6 line-box">
                <svg
                  class="S1-D6-SVG"
                  width="100%"
                  @click="channelLineClick(6)"
                  height="16"
                  viewBox="0 0 220 16"
                  preserveAspectRatio="none"
                >
                  <path
                    d="M0,0 Q110,16, 220,0"
                    :stroke="channels.includes(6) ? '#00D008' : '#848484'"
                    fill="transparent"
                    stroke-width="3"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="eig-card view-setting-card">
        <p class="card-title">View Setting</p>
        <div class="eig-card view-setting-box">
          <p class="card-title">Plot Type</p>
          <div class="view-setting">
            <div class="view-setting-radio">
              <a-radio-group @change="handleChange" v-model:value="radioValue">
                <a-radio :style="radioStyle" :value="1">Raw Data</a-radio>
                <a-radio :style="radioStyle" :value="2"
                  >Optical Density</a-radio
                >
                <a-radio :style="radioStyle" :value="3">Concentration</a-radio>
              </a-radio-group>
            </div>
            <div>
              <a-checkbox-group
                class="view-setting-checkbox-group"
                v-if="radioValue !== 3"
                v-model:value="checkboxValue1"
                @change="handleChange"
                style="width: 100%"
              >
                <a-checkbox value="1">735nm</a-checkbox>
                <a-checkbox value="2">805nm</a-checkbox>
                <a-checkbox value="3">850nm</a-checkbox>
              </a-checkbox-group>
              <a-checkbox-group
                class="view-setting-checkbox-group"
                v-if="radioValue === 3"
                @change="handleChange"
                v-model:value="checkboxValue2"
                style="width: 100%"
              >
                <a-checkbox value="1">HbO</a-checkbox>
                <a-checkbox value="2">HbR</a-checkbox>
                <a-checkbox value="3">HbT</a-checkbox>
              </a-checkbox-group>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {
  ref,
  reactive,
  onMounted,
  nextTick,
  watch,
  onBeforeUnmount,
} from "vue";
// import { CustomDatabase } from "../../utils/db";
import * as echarts from "echarts";
const radioValue = ref<number>(1);
const checkboxValue1 = ref(["1"]);
const checkboxValue2 = ref(["1"]);
const selectionHeight = ref(0);
const selectionWidth = ref(0);
const seriesStep = ref(30);
const seriesMaxStep = 30;
const isRender = ref(false);
const minTimeGap = 250;
let isRenderTimer, realTimer;
const channels = ref([1, 2, 3, 4, 5, 6, 7, 8]);
import { CustomDatabase } from "../../utils/db";
import { useIndexStore } from "../../store/index";
import { CustomBluetooth } from "../../utils/bluetooth";
import { storeToRefs } from "pinia";
let pkgDataList: any = [];
let pkgSourceData: any = [];
let pkgMaxTime = 30;
const indexStore = useIndexStore();
const { play, recordId, playIndex, isDragSlider, isConnect } =
  storeToRefs(indexStore);
const db = new CustomDatabase();
let bluetooth = new CustomBluetooth();
let myChart: echarts.EChartsType;
let timerPlay, timer;
interface showSeriesDataType {
  chanIndex: number;
  index: number;
  type: string;
  radioIndex: number;
  name: string;
  color: string;
}
let showSeriesData: showSeriesDataType[] = [];
const seriesRDNames = [
  "S1D5.λ1",
  "S1D5.λ2",
  "S1D5.λ3",
  "S1D6.λ1",
  "S1D6.λ2",
  "S1D6.λ3",
  "S1D7.λ1",
  "S1D7.λ2",
  "S1D7.λ3",
  "S1D8.λ1",
  "S1D8.λ2",
  "S1D8.λ3",
  "S2D1.λ1",
  "S2D1.λ2",
  "S2D1.λ3",
  "S2D2.λ1",
  "S2D2.λ2",
  "S2D2.λ3",
  "S2D3.λ1",
  "S2D3.λ2",
  "S2D3.λ3",
  "S2D4.λ1",
  "S2D4.λ2",
  "S2D4.λ3",
];
const seriesODNames = [
  "S1D5.λ1",
  "S1D5.λ2",
  "S1D5.λ3",
  "S1D6.λ1",
  "S1D6.λ2",
  "S1D6.λ3",
  "S1D7.λ1",
  "S1D7.λ2",
  "S1D7.λ3",
  "S1D8.λ1",
  "S1D8.λ2",
  "S1D8.λ3",
  "S2D1.λ1",
  "S2D1.λ2",
  "S2D1.λ3",
  "S2D2.λ1",
  "S2D2.λ2",
  "S2D2.λ3",
  "S2D3.λ1",
  "S2D3.λ2",
  "S2D3.λ3",
  "S2D4.λ1",
  "S2D4.λ2",
  "S2D4.λ3",
];
const seriesConcNames = [
  "S1D5.HbO",
  "S1D5.HbR",
  "S1D5.HbT",
  "S1D6.HbO",
  "S1D6.HbR",
  "S1D6.HbT",
  "S1D7.HbO",
  "S1D7.HbR",
  "S1D7.HbT",
  "S1D8.HbO",
  "S1D8.HbR",
  "S1D8.HbT",
  "S2D1.HbO",
  "S2D1.HbR",
  "S2D1.HbT",
  "S2D2.HbO",
  "S2D2.HbR",
  "S2D2.HbT",
  "S2D3.HbO",
  "S2D3.HbR",
  "S2D3.HbT",
  "S2D4.HbO",
  "S2D4.HbR",
  "S2D4.HbT",
];
const seriesColors = [
  "rgba(137,137,140,1)",
  "rgba(252,215,131,1)",
  "rgba(150,224,185,1)",
  "rgba(90,205,204,1)",
  "rgba(203,179,237,1)",
  "rgba(250,228,148,1)",
  "rgba(192,192,192,1)",
  "rgba(156,200,223,1)",
];

let xAxisArr = reactive<echarts.EChartOption.XAxis[]>([]);
let yAxisArr = reactive<echarts.EChartOption.YAxis[]>([]);
let seriesArr = reactive<echarts.EChartOption.Series[]>([]);
let gridArr = reactive<echarts.EChartOption.Grid[]>([]);

let seriesRDNameObj: showSeriesDataType[];
let seriesODNameObj: showSeriesDataType[];

let seriesConcNameObj: showSeriesDataType[];
const radioStyle = reactive({
  display: "flex",
  height: "30px",
  lineHeight: "30px",
});

watch(
  play,
  (newValue) => {
    if (newValue) {
      timerPlay = setInterval(() => {
        if (pkgSourceData.length) {
          if (
            playIndex.value * minTimeGap >=
            pkgSourceData[pkgSourceData.length - 1].time_mark
          ) {
            timerPlay && clearInterval(timerPlay);
            return;
          }
          pkgDataList = joinPkgList();  
          renderData();
        }
      }, 250);
    } else {
      timerPlay && clearInterval(timerPlay);
    }
  },
  {
    immediate: true,
  }
);

watch(isRender, (newValue) => {
  if (newValue) {
    realTimerRenderData();
  } else {
    realTimer && clearInterval(realTimer);
  }
});

watch(isDragSlider, (newValue) => {
  // 拖拽了进度条更改渲染数据
  if (newValue) {
    indexStore.isDragSlider = false;
    // handleOldData();
  }
});
watch(recordId, (value) => {
  if (value !== undefined) {
    initialize();
  }
});

watch(isConnect, (newValue) => {
  if (newValue) {
    pkgDataList = [];
  }
});

onMounted(function () {
  // var sq = new CustomDatabase()
  // console.log(sq,"sq");
  nextTick(() => {
    getSelectionHeight();
  });
  initialize();
});

onBeforeUnmount(() => {
  bluetooth.removeNotice(bluetoothNotice);
  timer && clearInterval(timer);
  timerPlay && clearInterval(timerPlay);
  realTimer && clearInterval(realTimer);
  isRenderTimer && clearTimeout(isRenderTimer);
});

// 获取元素高度
const getSelectionHeight = () => {
  selectionHeight.value =
    document.getElementById("selection")?.clientHeight || 0;
  selectionWidth.value = document.getElementById("selection")?.clientWidth || 0;
};

// 蓝牙数据通知
const bluetoothNotice = (data) => {
  handlePkgList(data);
  // handleRealTimeData(blueToothdataMapping(data));
  isRender.value = true;
  isRenderTimer && clearTimeout(isRenderTimer);
  isRenderTimer = setTimeout(() => {
    isRender.value = false;
  }, 4 * 1000);
};

// 定时渲染
const realTimerRenderData = () => {
  realTimer && clearInterval(realTimer);
  realTimer = setInterval(() => {
    renderData();
  }, minTimeGap);
};

// 渲染
const renderData = () => {
  generateSeries();
  initSeries();
};

// 数据包处理
const handlePkgList = (data) => {
  if (
    pkgDataList.length &&
    pkgDataList[pkgDataList.length - 1].time_mark - pkgDataList[0].time_mark >
      pkgMaxTime * 1000
  ) {
    pkgDataList.shift();
  }
  //  有IR标识
  if (data.pkg_type === 2) {
    pkgDataList.push(data);
  }
};

// 判断是否加入数据包队列
const joinPkgList = () => {
  let tempPkgDataList: any = [];
  if (!pkgSourceData.length) {
    return [];
  }
  for (let index = 0; index < pkgSourceData.length; index++) {
    const item = pkgSourceData[index];
    if (
      item.time_mark - pkgSourceData[0].time_mark <=
        playIndex.value * minTimeGap &&
      item.pkg_type === 2
    ) {
      tempPkgDataList.push(item);
    } 
  }
  return tempPkgDataList;
};

const initialize = () => {
  pkgSourceData = [];
  if (!recordId.value) {
    bluetooth.addNotice(bluetoothNotice);
  } else {
    bluetooth.removeNotice(bluetoothNotice);
    db.get(`select sourceData from record where id = ${recordId.value}`).then(
      (res) => {
        pkgSourceData = JSON.parse(res.sourceData);
      }
    );
  }

  initData();
  nextTick(() => {
    myChart = echarts.init(document.getElementById("series"));
    initSeries();
  });
};

// 配置改变
const handleChange = () => {
  myChart.clear();
  generateShowSeriesData();
  initSeries();
};

const changeAlpha = (colorString: string, newAlpha: string) => {
  // 使用正则表达式替换alpha值
  return colorString.replace(/[^,]+(?=\))/, newAlpha);
};

// 生成颜色
const generateColor = (colorStr: string, index: number) => {
  if (index === 0) {
    return changeAlpha(colorStr, "0.8");
  }
  if (index === 1) {
    return changeAlpha(colorStr, "0.9");
  }
  return colorStr;
};

// 初始化数据集
const initData = () => {
  seriesRDNameObj = seriesRDNames.map((item, index) => {
    return {
      chanIndex: Math.ceil((index + 1) / 3),
      radioIndex: (index % 3) + 1,
      index: index,
      type: "RD",
      name: item,
      color: generateColor(
        seriesColors[Math.ceil((index + 1) / 3) - 1],
        index % 3
      ),
    };
  });
  seriesODNameObj = seriesODNames.map((item, index) => {
    return {
      chanIndex: Math.ceil((index + 1) / 3),
      radioIndex: (index % 3) + 1,
      index: index,
      type: "OD",
      name: item,
      color: generateColor(
        seriesColors[Math.ceil((index + 1) / 3) - 1],
        index % 3
      ),
    };
  });
  seriesConcNameObj = seriesConcNames.map((item, index) => {
    return {
      chanIndex: Math.ceil((index + 1) / 3),
      radioIndex: (index % 3) + 1,
      index: index,
      type: "Conc",
      name: item,
      color: generateColor(
        seriesColors[Math.ceil((index + 1) / 3) - 1],
        index % 3
      ),
    };
  });
  generateShowSeriesData();
};

// 生成需要显示的基础模版数据
const generateShowSeriesData = () => {
  let templateData: showSeriesDataType[] = [];
  let checkboxValue: string[] = [];
  if (radioValue.value === 1) {
    templateData = seriesRDNameObj;
    checkboxValue = checkboxValue1.value;
  } else if (radioValue.value === 2) {
    checkboxValue = checkboxValue1.value;
    templateData = seriesODNameObj;
  } else if (radioValue.value === 3) {
    checkboxValue = checkboxValue2.value;
    templateData = seriesConcNameObj;
  }
  showSeriesData = templateData.filter((item) => {
    return (
      channels.value.includes(item.chanIndex) &&
      checkboxValue.includes(item.radioIndex + "")
    );
  });
  console.log("showSeriesData", showSeriesData);

  generateXAxisArr();
  generateYAxisArr();
  generateGrid();
  generateSeries();
};

// 生成X轴
const generateXAxisArr = () => {
  xAxisArr = showSeriesData.map((item, index) => {
    return {
      type: "time",
      show: index === showSeriesData.length - 1,
      boundaryGap: false,
      gridIndex: index,
      max: seriesStep.value * 1000,
      axisLabel: {
        color: "#666666",
        formatter: function (value, index) {
          return echarts.format.formatTime("s", value);
        },
        showMaxLabel: true,
        showMinLabel: true,
      },
      axisLine: {
        lineStyle: {
          color: "#dddddd",
        },
      },
    };
  });
};
// 生成y轴
const generateYAxisArr = () => {
  yAxisArr = showSeriesData.map((item, index) => {
    return {
      show: true,
      axisLine: {
        show: false, // 显示y轴线
      },
      axisLabel: {
        //坐标轴刻度标签
        show: false,
      },
      axisTick: {
        show: false, // 可取消y轴刻度线
      },
      splitLine: {
        show: false, // 去除网格线
      },
      nameRotate: 0,
      name: item.name,
      nameLocation: "middle",
      nameTextStyle: {
        fontSize: 12,
        fontWeight: "bold",
      },
      type: "value",
      gridIndex: index,
    };
  });
};

// 生成grid
const generateGrid = () => {
  gridArr = showSeriesData.map((item, index) => {
    return {
      left: "7%",
      right: "4%",
      top: (100 / showSeriesData.length) * index + "%",
      bottom: 100 - (100 / showSeriesData.length) * (index + 1) + "%",
      containLabel: false,
    };
  });
};

// 生成series
const generateSeries = () => {
  // let tempSeriesData;
  // tempSeriesData = Object.assign([], seriesData.slice(-seriesStep.value * 4));

  // seriesData
  // [{
  //   RD:[`通道1-735nm，通道1-805nm，通道1-850nm，通道2...`],
  //   OD:[`通道1-735nm，通道1-805nm，通道1-850nm，通道2...`],
  //   Conc:[`通道1-HbO，通道1-HbR，通道1-HbT，通道2...`]
  // },{
  //   RD:[`通道1-735nm，通道1-805nm，通道1-850nm，通道2...`],
  //   OD:[`通道1-735nm，通道1-805nm，通道1-850nm，通道2...`],
  //   Conc:[`通道1-HbO，通道1-HbR，通道1-HbT，通道2...`]
  // }]
  // 构建后
  // {
  // 通道1-735nm / 通道1-HbO
  // RD:['通道1-735nm','通道1-735nm']
  // OD:['通道1-735nm','通道1-735nm']
  // Conc:['通道1-HbO','通道1-HbO']
  // }
  seriesArr = showSeriesData.map((item, index) => {
    let tempObj: any = {
      RD: [],
      OD: [],
      Conc: [],
    };

    tempObj.RD = conversionPkgtoTimeSeries(
      mapChanToField(item.chanIndex),
      mapRadioToField(item.radioIndex),
      seriesStep.value
    );

    tempObj.OD = conversionPkgtoTimeSeries(
      mapChanToField(item.chanIndex),
      mapRadioToField(item.radioIndex),
      seriesStep.value
    );
    tempObj.Conc = conversionPkgtoTimeSeries(
      mapChanToField(item.chanIndex),
      mapRadioToField(item.radioIndex),
      seriesStep.value
    );
    return {
      data: tempObj[item.type],
      type: "line",
      name: item.name,
      symbol: "none",
      lineStyle: {
        width: 1,
        color: item.color,
      },
      markLine: {
        symbol: ["none", "none"],
        label: {
          show: false,
        },
        lineStyle: {
          type: "solid",
          width: 2,
          color: "#FFECE4",
        },
        // data: [{ xAxis: 3 }],
      },
      xAxisIndex: index,
      yAxisIndex: index,
    };
  });
};

const mapChanToField = (index) => {
  return index - 1;
  switch (
    index // 通道索引
  ) {
    case 1:
      return "near_infrared";
    case 2:
      return "near_infrared";
    case 3:
      return "near_infrared";
    default:
      return "near_infrared";
  }
};
const mapRadioToField = (index) => {
  switch (
    index // 波形索引
  ) {
    case 1:
      return 0;
    case 2:
      return 2;
    case 3:
      return 1;
    default:
      return 0;
  }
};

const conversionPkgtoTimeSeries = (field, index, step) => {
  if (pkgDataList.length < 1) return [];
  let maxTimer = pkgDataList[pkgDataList.length - 1].time_mark;
  let minTime = maxTimer - step * 1000;
  let sliceData = pkgDataList.filter(
    (item) => item.time_mark >= minTime && item.time_mark <= maxTimer
  );
  let baseTime = 0;
  return sliceData.map((item, sliceIndex) => {
    if (sliceIndex !== 0) {
      baseTime += item.time_mark - sliceData[sliceIndex - 1].time_mark;
    }
    return {
      value: [baseTime, item.near_infrared[field][index]],
    };
  });
};

// 渲染图表
const initSeries = () => {
  myChart?.setOption({
    animation: false,
    xAxis: xAxisArr,
    grid: gridArr,
    yAxis: yAxisArr,
    series: seriesArr,
  },{
    lazyUpdate: true,
  });
};

// 切换渠道
const channelLineClick = (value: number | Array<number>) => {
  if (Array.isArray(value)) {
    let flag = value.every((item) => {
      return channels.value.includes(item);
    });
    if (flag) {
      channels.value = channels.value.filter((item) => {
        return !value.includes(item);
      });
    } else {
      value.forEach((item) => {
        if (!channels.value.includes(item)) {
          channels.value.push(item);
        }
      });
    }
  } else {
    let index = channels.value.find((item) => {
      return item === value;
    });
    if (index) {
      channels.value = channels.value.filter((item) => {
        return item !== value;
      });
    } else {
      channels.value.push(value);
    }
  }

  handleChange();
};
</script>
<style scoped></style>
