---
title: 基于ESP32移动环境与位置云端记录仪
categories:
  - 项目合集
top_img: ../img/index.jpg
---

# 移动环境与位置云端记录仪 项目笔记

## 一、项目简介

本项目基于 ESP32 微控制器，整合温湿度、光照、GPS定位等多种传感器，实现本地环境与位置信息的采集，并通过 MQTT 协议定时上传至华为云 IoTDA 平台。后端服务器可通过华为云北向API定时拉取设备属性数据，并通过 WebSocket 推送给网页前端，最终实现实时可视化展示。适合环境监测、物联网课程实践、小型物联云端数据展示等应用场景。

---

## 二、主要软硬件组件

- **主控板**：ESP32开发板
- **温湿度传感器**：DHT11（或DS18B20温度模块）
- **光敏电阻传感器**：模拟光照传感器，AO口接ESP32 ADC引脚
- **GPS模块**：NEO-6M，串口TTL通讯
- **云平台**：华为云IoTDA，支持MQTT直连、属性上报、设备影子
- **Web服务器/前端**：uni-app H5网页（或Android、微信小程序）
- **后端**：Node.js (Express + WebSocket)，定时通过北向API拉取设备属性并推送

---

## 三、硬件连接说明

- **DHT11数据引脚** → ESP32 GPIO4  
- **光敏AO引脚** → ESP32 GPIO34 (或2/4/5/18/19/15等ADC口)
- **GPS模块TX** → ESP32 GPIO17（作为RX2接收，波特率38400）
- **GPS模块RX** 可不接（或接ESP32 TX2: GPIO16）
- 所有传感器共地

---

## 四、ESP32主控代码简要逻辑

1. 定时读取DHT11温湿度、光敏电阻ADC值、GPS数据
2. 解析NMEA协议，提取经纬度、卫星数等信息
3. 拼接JSON属性包，MQTT publish到华为云 IoTDA 设备属性上报Topic  
   Topic格式：`$oc/devices/{device_id}/sys/properties/report`
4. 支持掉线重连与异常处理

---

## 五、华为云IoTDA平台配置

1. 创建产品，定义服务与属性（如temperature, humidity, light, longitude, latitude, satellites）
2. 创建设备，获取`device_id`，开通MQTT直连
3. 记录实例API调用地址、项目ID（projectId）、服务ID（serviceId）

---

## 六、Node.js后端（北向API+WebSocket）

1. 定时用API获取token，拉取设备影子属性  
   API路径:GET 
   ```
   https://{iotda实例域名}/v5/iot/{project_id}/devices/{device_id}/shadow

   ```

2. 解析`reported.properties`，存为`latestData`
3. WebSocket推送：有前端连接后，每次属性更新即主动推送
4. 供H5前端用WebSocket方式实时订阅

---

## 七、前端网页H5

1. WebSocket连接（如 `wss://xxx.ngrok-free.app` 或服务器公网地址）
2. 收到属性JSON后自动展示到页面
3. 支持手动切换WebSocket域名、断开与重连
4. 展示数据项：温度、湿度、光照、经纬度、卫星数

---

## 八、虚拟机/服务器部署简要流程

1. **网页前端**：uni-app打包为H5，`python3 -m http.server 8080` 或 `nginx` 运行
2. **Node.js后端**：用`express`+`ws`监听WebSocket端口并跑北向API拉数据
3. **公网暴露**：可用ngrok映射http/ws端口（注意ngrok免费版仅单端口），或直接云服务器公网访问
4. **本地/虚拟机调试**：防火墙开放端口，ngrok/服务器保证外部可访问

---

## 九、常见问题与注意事项

- ngrok免费版**只支持单端口**，如需同时公网暴露前后端，推荐合并端口或用云服务器
- WebSocket与HTTP协议/端口要完全对应，H5网页端用`wss://`，本地开发可用`ws://`
- 华为云北向API必须用有效token、正确的projectId、deviceId、实例API域名，否则会404或401
- 手机APP端WebSocket受证书影响可能不能用ngrok（H5网页推荐，无证书警告）
- 设备端属性要及时上报，云端设备影子要能实时刷新
- 若前端收不到数据，优先排查后端WebSocket推送与前端WebSocket地址是否一致

---

## 十、典型代码片段参考

### Node.js静态文件+WebSocket合并服务

```js
const express = require('express');
const http = require('http');
const WebSocket = require('ws');
const path = require('path');

const app = express();
const server = http.createServer(app);
const wss = new WebSocket.Server({ server });

app.use(express.static(path.join(__dirname, 'h5')));

wss.on('connection', ws => {
ws.send(JSON.stringify({ msg: "hello" }));
});

server.listen(3000, () => {
console.log('服务已启动: http://localhost:3000');
});

//前端H5 WebSocket监听

let ws = new WebSocket('wss://xxxx.ngrok-free.app');
ws.onopen = () => { console.log('连接成功'); };
ws.onmessage = e => { console.log('收到', e.data); };
```



