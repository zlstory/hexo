---
layout: post
title: " Vue组件中eventBus使用"
tags: [vue]
date: 2019-9-17
comments: true
---


作用：在要相互通信的兄弟组件之中，都引入一个新的vue实例，然后通过分别调用这个实例的事件触发和监听来实现通信和参数传递。

实现：


```javascript
//新建 bus.js
import Vue from "vue";
export default new Vue();
```



```javascript
//sender

import Bus from "@/utils/bus.js";
  destroyed() {
    Bus.$emit("getChannleMsg", msg);
  }
```



```javascript
//Receiver
import Bus from "@/utils/bus.js";
  created() {
    Bus.$off("getChannleMsg");
    Bus.$on("getChannleMsg", this.getMsg);
  },

  methods:{
    getMsg(val) {
      this.data = val;
    },
  }
```
