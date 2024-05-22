## 项目

QQ机器人开发

## QQ开放平台

[官网入口](https://q.qq.com)

### QQ机器人文档

[API文档](https://bot.q.qq.com/wiki/develop/api-v2/)

### 官方SDK

- [Node SDK 文档](https://bot.q.qq.com/wiki/develop/nodesdk/)
- [Node SDK 源码](https://github.com/tencent-connect/bot-node-sdk) 

***

- [Python SDK 文档](https://bot.q.qq.com/wiki/develop/pythonsdk/)
- [Python SDK 源码](https://github.com/tencent-connect/botpy) 


***

- [Go SDK 文档](https://bot.q.qq.com/wiki/develop/gosdk/)
- [Go SDK 源码](https://github.com/tencent-connect/botgo) 


### 开源SDK

由于官方SDK无人维护，且没有提供群消息相关方法，所以选择使用个人维护的开源SDK

- [源码](https://github.com/feilongproject/QQNodeSDK) 

- 安装

```cmd
npm i qq-bot-sdk
```

- 引用

```javascript
const { createOpenAPI, createWebsocket, AvailableIntentsEventsEnum } = require("qq-bot-sdk"); // commonjs引用方法

const testConfigWs = {
    appID: "APPID",
    token: "TOKEN",
    intents: [AvailableIntentsEventsEnum.GUILD_MESSAGES], // 设置监听类型
};

const client = createOpenAPI(testConfigWs); // 创建client实例（用于发送消息）
const ws = createWebsocket(testConfigWs); // 创建ws实例（用于接收消息）
```

- AGPL-3.0 license
