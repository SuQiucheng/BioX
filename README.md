# Vite+Vue3+Electron+Typescript template

![screenshot](./src/assets/screenshot.png)

## Get Started

### 1.环境

#### node  v16.13.2
#### python 3.6.0

```bash
npm config edit
```

#### 该命令会打开npm的配置文件，请在空白处添加

```bash
registry=https://registry.npmmirror.com
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
ELECTRON_BUILDER_BINARIES_MIRROR=https://npmmirror.com/mirrors/electron-builder-binaries/

```
### 2. install packages

```bash
npm i
```

### 3.  run

```bash
npm run app:dev
```



### 注
```
shims.d文件
解决router 找不到模块“XXX.vue”或其相应的类型声明。ts(2307)(发生在rrouter.ts)
```