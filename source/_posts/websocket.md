---
layout: post
title: "在vue-cli中使用websocket"
tags: [vue，websocket]
date: 2019-10-3
comments: true
---


1. 什么是websocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

2. 为什么要使用websocket

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

3. 在vue-cli项目中使用websocket

```javascript

// app.vue

<script>

export default {
  name: "App",
  data() {
    
  },

  data() {
    return {
      websock: null,
      reconnectData: null,
      lockReconnect: false, //避免重复连接，因为onerror之后会立即触发 onclose
      timeout: 1000, //10s一次心跳检测
      timeoutObj: null,
      serverTimeoutObj: null,
      socketData: {
        pageName: this.GLOBAL.pageName,
        futuresName: this.GLOBAL.futuresName,
        enterTime: this.GLOBAL.enterTime,
        leaveTime: this.GLOBAL.leaveTime
      }
    };
  },
  methods: {
    initWebSocket() {
      console.log("启动中");
      let wsurl = `ws://47.56.99.230/api/websocket/${this.userId}`;
      this.websock = new WebSocket(wsurl);
      this.websock.onopen = this.websocketonopen; //连接成功
      this.websock.onmessage = this.websocketonmessage; //广播成功
      this.websock.onerror = this.websocketonerror; //连接断开，失败
      this.websock.onclose = this.websocketclose; //连接关闭
    }, //初始化weosocket
    websocketonopen() {
      console.log("连接成功");
      // this.heatBeat();
    }, //连接成功
    websocketonerror() {
      console.log("连接失败");
      this.reconnect();
    }, //连接失败
    websocketclose() {
      console.log("断开连接");
      this.reconnect();
    }, //各种问题导致的 连接关闭
    websocketonmessage(data) {
      // this.heatBeat(); //收到消息会刷新心跳检测，如果一直收到消息，就推迟心跳发送
      const messageType = JSON.parse(data.data).type;
      if (messageType == "flush_user_message") {
        this.$store.commit("SET_USERMESSAGESTATUS", "true");
      }
    }, //数据接收
    websocketsend(data) {
      this.websock.send(
        JSON.stringify({
          type: "add_browse_record",
          data: {
            pageName: this.GLOBAL.pageName,
            futuresName: this.GLOBAL.futuresName,
            enterTime: this.GLOBAL.enterTime,
            leaveTime: this.GLOBAL.leaveTime
          }
        })
      );
    },  //数据发送
   
    reconnect() {
      if (this.lockReconnect || !this.userId) {
        //这里很关键，因为连接失败之后之后会相继触发 连接关闭，不然会连接上两个 WebSocket
        return;
      }
      this.lockReconnect = true;
      this.reconnectData && clearTimeout(this.reconnectData);
      this.reconnectData = setTimeout(() => {
        this.initWebSocket();
        this.lockReconnect = false;
      }, 1000);
    }, //socket重连
    heatBeat() {
      this.timeoutObj && clearTimeout(this.timeoutObj);
      this.serverTimeoutObj && clearTimeout(this.serverTimeoutObj);
      this.timeoutObj = setTimeout(() => {
        // this.websocketsend({
        //   type: "add_browse_record",
        //   data: this.socketData
        // }); //根据后台要求发送
        this.serverTimeoutObj = setTimeout(() => {
          this.websock.close(); //如果  5秒之后我们没有收到 后台返回的心跳检测数据 断开socket，断开后会启动重连机制
        }, 5000);
      }, this.timeout);
    },
    fetchData() {
      if (this.userId) {
        console.log;
        this.websocketsend({
          type: "add_browse_record",
          data: {
            pageName: this.GLOBAL.pageName,
            futuresName: this.GLOBAL.futuresName,
            enterTime: this.GLOBAL.enterTime,
            leaveTime: this.GLOBAL.leaveTime
          }
        });
      }
    },
   
  created() {
    if (this.userId) { //如果是登陆状态再触发websocket
      this.initWebSocket();
    }
  },
  watch: {
    $route(to, from) {
      if (to.path == "/" && from.path == "/login") {
        this.initWebSocket();
        return;
      }
      if (this.userId) {
        this.fetchData();
      }
    }
  },
  destroyed() {
    this.lockReconnect = true;
    // this.websock.close(); //离开路由之后断开websocket连接
    clearTimeout(this.reconnectData); //离开清除 timeout
    clearTimeout(this.timeoutObj); //离开清除 timeout
    clearTimeout(this.serverTimeoutObj); //离开清除 timeout
  }
};
</script>

```
