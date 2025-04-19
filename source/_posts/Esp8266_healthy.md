---
title: 基于ESP8266将健康数据上传华为云平台进行实时监控
categories:
  - 项目合集
top_img: ../img/index.jpg
---

# 一、基于ESP8266的健康数据上传项目
## （1）项目目标与背景
本项目旨在搭建一个实时健康数据监测系统，通过传感器采集数据，利用ESP8266主控板传输数据至华为云平台，并开发前端界面实现数据可视化。随着人们对健康管理的重视，此类实时、便捷的健康监测系统需求日益增长。

## （2）项目整体架构
项目由硬件采集端、数据传输链路、后端服务和前端展示四部分构成。硬件采集端负责收集数据，数据传输链路依托ESP8266的Wi-Fi功能，后端服务采用nodejs搭建，前端使用uniapp开发。

# 二、硬件搭建
## （1）ESP8266主控板
### ① 选型原因
ESP8266以其低成本、低功耗及强大的网络功能，成为本项目主控板的理想选择。它能够轻松实现数据的处理与网络传输，满足项目需求。

### ② 连接与配置
通过USB转TTL模块连接电脑，用于程序烧录与调试。在开发环境中，需配置好对应的端口和通信参数。

## （2）MAX30102传感器
### ① 功能作用
MAX30102用于精确采集心率和血氧饱和度数据，为健康监测提供关键指标。

### ② 硬件连接
采用I2C接口与ESP8266连接，VCC接3.3V供电，GND接地，SCL和SDA分别对应连接至ESP8266的I2C引脚，确保通信正常。

## （3）DHT11传感器
### ① 功能说明
DHT11主要用于测量环境温度和湿度，在此用作人体温度湿度检测，为健康数据提供环境参考。

### ② 连接方式
VCC接3.3V，GND接地，数据引脚连接至ESP8266的GPIO引脚，以便读取传感器数据。

## （4）I2C协议OLED显示屏
### ① 用途
实时展示采集到的健康数据和环境参数，方便现场查看。

### ② 硬件连接方式
1. **MAX30105心率血氧传感器**
    - **电源连接**：
      - MAX30105的VCC引脚 → ESP8266的3.3V引脚
      - MAX30105的GND引脚 → ESP8266的GND引脚
    - **通信连接**：
      - MAX30105的SDA引脚 → ESP8266的D2引脚（GPIO4）
      - MAX30105的SCL引脚 → ESP8266的D1引脚（GPIO5）
2. **DHT11温湿度传感器**
    - **电源连接**：
      - DHT11的VCC引脚 → ESP8266的3.3V或5V引脚
      - DHT11的GND引脚 → ESP8266的GND引脚
    - **通信连接**：
      - DHT11的DATA引脚 → ESP8266的D3引脚（GPIO0）
3. **OLED显示**：
SDA_PIN被重新定义为12（对应D6引脚），SCL_PIN保持为14（对应D5引脚），这样OLED就会使用D5和D6引脚进行通信。

# 三、软件实现
## （1）前端（uniapp）
### ① 界面设计
设计简洁直观的界面，包含实时数据展示区，以数字和图表形式展示心率、血氧、温度、湿度等数据；历史数据查看区，方便用户追溯过往健康数据。

### ② 通信实现
利用uniapp的网络请求功能，与后端服务器进行数据交互。通过HTTP请求获取华为云平台经后端处理后的实时健康数据，并在前端界面展示。
```bash
//index.vue
<template>
    <view class="container">
        <view class="header">
            <image class="logo" src="/static/health.png"></image>
            <text class="title">设备健康数据</text>
            <text class="connection-status">{{ connectionStatus }}</text>
        </view>
        <view class="data-container">
            <view class="data-item">
                <image class="icon" src="https://img.icons8.com/?size=96&id=78394&format=png" mode="widthFix"></image>
                <text class="label">心率:</text>
                <text class="value">{{ heart }}</text>
            </view>
            <view class="data-item">
                <image class="icon" src="https://img.icons8.com/?size=160&id=fbfmUNq5qEyS&format=png" mode="widthFix"></image>
                <text class="label">血氧:</text>
                <text class="value">{{ o2 }}</text>
            </view>
            <view class="data-item">
                <image class="icon" src="https://img.icons8.com/color/24/000000/thermometer.png" mode="widthFix"></image>
                <text class="label">温度:</text>
                <text class="value">{{ tem }}</text>
            </view>
            <view class="data-item">
                <image class="icon" src="https://img.icons8.com/color/24/000000/humidity.png" mode="widthFix"></image>
                <text class="label">湿度:</text>
                <text class="value">{{ hum }}</text>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            heart: '--',
            o2: '--',
            tem: '--',
            hum: '--',
            socketTask: null,
            connectionStatus: '未连接'
        };
    },
    onLoad() {
        this.connectWebSocket();
    },
    methods: {
        connectWebSocket() {
            this.socketTask = uni.connectSocket({
                url: 'wss://a87c-219-135-121-217.ngrok-free.app',
                success: () => {
                    console.log('WebSocket 连接成功');
                    this.connectionStatus = '已连接';
                }
            });

            this.socketTask.onOpen(() => {
                console.log('WebSocket 已打开');
            });

            this.socketTask.onMessage((res) => {
                const data = JSON.parse(res.data);
                this.heart = data.heart;
                this.o2 = data.o2;
                this.tem = data.tem;
                this.hum = data.hum;
            });

            this.socketTask.onError(() => {
                console.error('WebSocket 连接失败');
                this.connectionStatus = '连接失败';
            });

            this.socketTask.onClose(() => {
                console.log('WebSocket 已关闭');
                this.connectionStatus = '已断开';
            });
        }
    },
    onUnload() {
        if (this.socketTask) {
            this.socketTask.close();
        }
    }
};
</script>

<style>
.container {
    padding: 20px;
    background-color: #e6f7ff; /* 浅蓝色背景，符合健康清新感 */
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.header {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20px;
}

.logo {
    width: 60px;
    height: 60px;
    margin-bottom: 10px;
}

.title {
    font-size: 28px;
    font-weight: bold;
    color: #007AFF; /* 蓝色标题，突出主题 */
}

.connection-status {
    font-size: 16px;
    color: #666;
}

.data-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}

.data-item {
    width: 45%;
    margin-bottom: 15px;
    background-color: #fff;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.08);
    display: flex;
    align-items: center;
}

.icon {
    width: 24px;
    height: 24px;
    margin-right: 10px;
}

.label {
    font-size: 18px;
    font-weight: 500;
    color: #666;
    margin-right: 5px;
}

.value {
    font-size: 20px;
    font-weight: bold;
    color: #333;
}
</style>
```
## （2）后端（nodejs）
### ① 服务器搭建
基于Ngrok框架搭建服务器，在虚拟机ubantu22中暴露公网ip，配置路由以处理前端的各类请求，确保服务器稳定运行。

```
//虚拟机执行暴露公网ip 3000端口命令行
hjnb1314@hjnb1314-virtual-machine:~/桌面$ cd /usr/local/bin
hjnb1314@hjnb1314-virtual-machine:/usr/local/bin$ ./ngrok http 3000

```

### ② 数据处理
从华为云平台获取数据，进行数据清洗、格式转换等处理，再返回给前端。同时，接收ESP8266上传的数据，并存储至华为云平台，实现数据的双向流通。
```bash
//app.js
const WebSocket = require('ws');
const express = require('express');
const https = require('https');
const http = require('http');

const app = express();
const server = http.createServer(app);
const wss = new WebSocket.Server({ server });

const port = 3000;
let latestData = null;
let token = null;

// 替换为你的实际华为云账号信息
const authOptions = {
    hostname: 'iam.cn-north-4.myhuaweicloud.com',
    path: '/v3/auth/tokens',
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    }
};

const deviceOptions = {
    hostname: '设备示例http接入网址',
    path: `/v5/iot/填写项目id/devices/填写设备id/shadow`,
    method: 'GET'
};

function getToken() {
    // 请根据实际情况修改请求体中的内容
    const authData = JSON.stringify({
        "auth": {
            "identity": {
                "methods": ["password"],
                "password": {
                    "user": {
                        "name": "", // 替换为你的IMA用户名
                        "password": "", // 替换为你的IMA密码
                        "domain": {
                            "name": "" // 替换为你的用户名
                        }
                    }
                }
            },
            "scope": {
                "project": {
                    "name": "cn-north-4"
                }
            }
        }
    });

    const req = https.request(authOptions, (res) => {
        token = res.headers['x-subject-token'];
        console.log('获取到 Token:', token);
    });

    req.on('error', (error) => {
        console.error('获取 Token 时出错:', error);
    });

    req.write(authData);
    req.end();
}

function fetchData() {
    if (!token) {
        console.log('Token 未获取到，重新获取...');
        getToken();
        return;
    }

    const options = {
        ...deviceOptions,
        headers: {
            'Content-Type': 'application/json',
            'X-Auth-Token': token
        }
    };

    const req = https.request(options, (res) => {
        let data = '';
        res.on('data', (chunk) => { data += chunk; });
        res.on('end', () => {
            try {
                const responseData = JSON.parse(data);
                if (responseData?.shadow) {
                    const service = responseData.shadow.find(s => s.service_id === '01');
                    if (service?.reported?.properties) {
                        latestData = service.reported.properties;
                        console.log(`心率: ${latestData.heart}, 血氧: ${latestData.o2}, 温度: ${latestData.tem}, 湿度: ${latestData.hum}`);

                        // 通过 WebSocket 发送数据给前端
                        wss.clients.forEach(client => {
                            if (client.readyState === WebSocket.OPEN) {
                                client.send(JSON.stringify(latestData));
                            }
                        });
                    }
                }
            } catch (error) {
                console.error('解析错误:', error);
            }
        });
    });

    req.on('error', (error) => {
        console.error('请求错误:', error);
        // Token 可能过期，重新获取
        getToken();
    });
    req.end();
}

// 每 23 小时获取一次新的 Token
setInterval(getToken, 23 * 60 * 60 * 1000);
// 每 4 秒读取一次设备信息
setInterval(fetchData, 4000);

// 初始获取 Token
getToken();

server.listen(port, () => {
    console.log(`服务器运行在端口 ${port}`);
});

```
## （3）ESP8266程序
### ① 数据采集
编写代码，通过相应的库函数实现对MAX30102和DHT11传感器的数据读取，确保数据的准确性和实时
```bash
//4.0，test.ino

#include <Wire.h>
#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>
#include "MAX30105.h"
#include "spo2_algorithm.h"
#include "DHT.h"
#include <U8g2lib.h>

// 定义DHT11相关参数
#define D3 0
#define DHTPIN D3     // 选择一个未使用的引脚连接DHT11，这里用D3
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// 全局变量
float temperature;
float humidity;
int32_t o2;
int8_t validO2;
int32_t heart;
int8_t validHeart;

// WiFi设置
const char* ssid = "WIFI名称";
const char* password = "WIFI密码";

// 接收到命令后上发的响应topic
const char* Iot_link_MQTT_Topic_Report = "$oc/devices/设备ID/sys/properties/report";
const char* topic_Commands_Response = "$oc/devices/设备ID/sys/commands/response/request_id=";

// 华为云接入地址
const char* mqttServer = ""; //华为云matt接入hostname
// 接入端口
const int mqttPort = ;//输入mqtt接入端口

// MQTT连接参数
const char* clientId = "";//设备MQTTcilentId
const char* mqttUser = "";//设备MQTT UserName
const char* mqttPassword = "";//设备MQTT Password

// 定义一个安全的WiFi客户端用于MQTTS连接
WiFiClientSecure espClient;
// 定义MQTT客户端
PubSubClient client(espClient);

// 手动定义 I2C_SPEED_FAST，如果库中没有定义
#ifndef I2C_SPEED_FAST
#define I2C_SPEED_FAST 400000
#endif

MAX30105 particleSensor;

// 避免 BUFFER_SIZE 重复定义，修改为 MY_BUFFER_SIZE
#define MY_BUFFER_SIZE 50

uint32_t irBuffer[MY_BUFFER_SIZE];
uint32_t redBuffer[MY_BUFFER_SIZE];

// 设置u8g2对象，根据实际的OLED型号和I2C地址进行修改
U8G2_SSD1306_128X64_NONAME_F_SW_I2C u8g2(U8G2_R0, /* clock=*/ 14, /* data=*/ 12, /* reset=*/ U8X8_PIN_NONE); 

void setup() {
    Serial.begin(115200);
    // 连接WiFi网络
    WiFi.begin(ssid, password);
    Serial.print("Connecting to ");
    Serial.print(ssid);
    Serial.println(" ...");

    int i = 0;
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.print(i++);
        Serial.print(' ');
    }

    Serial.println("");
    Serial.println("Connection established!");
    Serial.print("IP address:    ");
    Serial.println(WiFi.localIP());

    espClient.setInsecure(); // 忽略SSL证书验证，实际使用建议配置正确证书
    client.setServer(mqttServer, mqttPort);

    if (!particleSensor.begin(Wire, I2C_SPEED_FAST)) {
        Serial.println("传感器未找到");
        while (1);
    }

    byte ledBrightness = 0x1F;
    byte sampleAverage = 4;
    byte ledMode = 2;
    byte sampleRate = 200;
    int pulseWidth = 411;
    int adcRange = 4096;

    particleSensor.setup(ledBrightness, sampleAverage, ledMode, sampleRate, pulseWidth, adcRange);
    particleSensor.setPulseAmplitudeRed(0x1F);
    particleSensor.setPulseAmplitudeIR(0x1F);

    // 初始化DHT11传感器
    dht.begin();

    // 初始化OLED
    u8g2.begin(); 
    u8g2.setFont(u8g2_font_ncenB08_tr); 
}

void MQTT_Init() {
    // MQTT服务器连接部分
    client.setServer(mqttServer, mqttPort);
    client.setKeepAlive(120);
    while (!client.connected()) {
        Serial.println("Connecting to MQTT...");
        if (client.connect(clientId, mqttUser, mqttPassword)) {
            Serial.println("connected");
        } else {
            Serial.print("failed with state ");
            Serial.print(client.state());
            delay(1000);
        }
    }
}

void MQTT_POST() {
    char jsonBuf[256]; // 增大缓冲区大小以容纳新属性
    // 修改服务ID和属性名，添加温度和湿度属性
    sprintf(jsonBuf, "{\"services\":[{\"service_id\":\"01\",\"properties\":{\"heart\":%d,\"o2\":%d,\"tem\":%.2f,\"hum\":%.2f}}]}", heart, o2, temperature, humidity);
    client.publish(Iot_link_MQTT_Topic_Report, jsonBuf);

    Serial.println(Iot_link_MQTT_Topic_Report);
    Serial.println(jsonBuf);
    Serial.println("Publish OK!");
}

void displayDataOnOLED() {
    u8g2.clearBuffer(); 

    // 设置较小的字体，这里假设存在合适的小字体，如u8g2_font_ncenB08_tr
    u8g2.setFont(u8g2_font_ncenB08_tr);

    // 第一行显示“HR”（心率 Heart Rate 的缩写）
    u8g2.drawStr(0, 10, "HR: ");
    char heartRateStr[5];
    itoa(heart, heartRateStr, 10);
    u8g2.drawStr(40, 10, heartRateStr);
    u8g2.drawStr(70, 10, "bpm");

    // 第二行显示“SpO₂”（血氧 SpO₂ 的常见表示）
    u8g2.drawStr(0, 22, "SpO₂: ");
    char bloodOxygenStr[5];
    itoa(o2, bloodOxygenStr, 10);
    u8g2.drawStr(40, 22, bloodOxygenStr);
    u8g2.drawStr(70, 22, "%");

    // 第三行显示“Tmp”（温度 Temperature 的缩写）
    u8g2.drawStr(0, 34, "Tmp: ");
    char temperatureStr[5];
    dtostrf(temperature, 4, 1, temperatureStr);
    u8g2.drawStr(40, 34, temperatureStr);
    u8g2.drawStr(70, 34, "°C");

    // 第四行显示“Hmd”（湿度 Humidity 的缩写）
    u8g2.drawStr(0, 46, "Hmd: ");
    char humidityStr[5];
    dtostrf(humidity, 4, 1, humidityStr);
    u8g2.drawStr(40, 46, humidityStr);
    u8g2.drawStr(70, 46, "%");

    u8g2.sendBuffer(); 
}

void loop() {
    for (byte i = 0; i < MY_BUFFER_SIZE; i++) {
        while (particleSensor.available() == false)
            particleSensor.check();

        redBuffer[i] = particleSensor.getRed();
        irBuffer[i] = particleSensor.getIR();
        particleSensor.nextSample();
    }

    maxim_heart_rate_and_oxygen_saturation(irBuffer, MY_BUFFER_SIZE, redBuffer, &o2, &validO2, &heart, &validHeart);

    // 读取DHT11数据
    temperature = dht.readTemperature();
    humidity = dht.readHumidity();

    // 使用 isnan 检查读取结果是否有效
    if (irBuffer[0] > 20000 && validO2 && validHeart && heart > 30 && heart < 250 && o2 > 50 && 
        !isnan(temperature) && !isnan(humidity)) {
        Serial.print("心率:");
        Serial.print(heart);
        Serial.print(",血氧:");
        Serial.print(o2);
        Serial.print(",温度:");
        Serial.print(temperature);
        Serial.print(",湿度:");
        Serial.println(humidity);

        if (!client.connected()) {
            MQTT_Init();
        }
        MQTT_POST();

        // 显示数据到OLED
        displayDataOnOLED();
    }

    delay(2000); // 每隔2秒上传一次数据
}    
```


# tip333

```
//具体项目文件可访问本人github仓库
https://github.com/hjnb1314/Esp8266_healthy_HuaweiCloud.git
```

# 以下为实物演示视频

<video src="../img/1.mp4" controls width="600"></video>
