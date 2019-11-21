---
layout: post
title: "mintLogin"
tags: [vue mint-ui]
date: 2019-4-26
comments: true
---

 使用vue+axios+vuex+mint-ui从0开始实现一个用户登录功能

### 安装所有环境

```javascript
npm install -g vue-cli

vue init webpack login

cd login

npm i mint-ui -S

npm install axios

npm install vuex --save

npm run dev

```
使用sass

```javascript

npm install node-sass  sass-loader --save-dev

```

使用vee-validate

```javascript
npm install vee-validate --save


import VeeValidate from 'vee-validate';
Vue.use(VeeValidate);


```

### 使用mint-ui

```javascript

import MintUI from 'mint-ui'

import 'mint-ui/lib/style.css'

Vue.use(MintUI)

```

### 页面逻辑

1. components新建login.vue搭一个简单的登录、注册、忘记密码页面,并设置好路由

```javascript
 routes: [
    {
      path: '/',
      name: 'Login',
      component: Login
    }
  ]

```
2. 封装axios

```javascript
//axios.js
import axios from 'axios'
import store from '../store'
import { Toast } from 'mint-ui'
import { Indicator } from 'mint-ui';
//创建axios实例
const service=axios.create({
  //baseURL:'',
  timeout:100000
})
// request拦截器
service.interceptors.request.use(
  config => {
    Indicator.open();
    return config
  },
  error => {
    Promise.reject(error)
  }
)
// response 拦截器
service.interceptors.response.use(
  response => {
    /**
     * status为非0是抛错 可结合自己业务进行修改
     */
    Indicator.close();
    const res = response.data
    if (res.status !== 0) {
      // 6:身份过期;
      if (res.status === 6) {
        MessageBox({
          title: '提示',
          message: '你已被登出,重新登录?',
          showCancelButton: true
        }).then(() => {
          store.dispatch('LogOut').then(() => {
            // 为了重新实例化vue-router对象 避免bug
            location.reload()
          })
        });
      }else{
        Toast({
          message: res.msg,
          iconClass: 'icon icon-error',
          duration: 5*1000
        });
        return response.data
      }
    } else {
      return response.data
    }
  },
  error => {
    Toast({
      message: error.message,
      iconClass: 'icon icon-success',
      duration: 5*1000
    });
    return Promise.reject(error)
  }
)

export default service

```

使用axios,新建login.js

```javascript
import request from '@/utils/request'
import Qs from 'qs'

export function login(username, password) {
  let formData = new FormData();
  formData.append('username', username);
  formData.append('password', password);
  //登录请求
  return request({
    url: '/api/user/login',
    method: 'post',
    headers:{'Content-Type':'application/x-www-form-urlencoded'},
    data: Qs.stringify({
      username,
      password
    })
  })
}
// 发送验证码
export function sendVerifyCode(type, phone) {
  return request({
    url: '/api/smsVerify',
    method: "post",
    data: Qs.stringify({
      smsType: type,
      mobile: phone
    })
  })
}

// 用户注册
export function register(username,password,vCode) {
  return request({
    url: "/api/register",
    method:"post",
    data:Qs.stringify({
      username:username,
      password:password,
      vCode:vCode
    })
  })
}



```
这里的/api是在webpack的config/index.js中的proxyTable配置好的

```javascript
 proxyTable: {
      '/api': {
        target: 'http://xxx.xxx.xx',
        changeOrigin: true,
        pathRewrite: {
          '^/api': '/api'
        }
      }
```

3. 使用vuex

新建store文件

```
├── index.html
├── main.js
├── service
│   └── ...               # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── getter.js        # 根级别的 action
    └── modules
        └── user.js      # 登录模块
```

```javascript
//user.js

import {
  login,
  logout
} from '@/api/login'

const user = {
  state: {
    userInfo: null
  },

  mutations: {
    SET_USERINFO_DATA(state, userInfo) {
      //在登录时需要存储的信息
      state.userInfo = userInfo
    },
  },

  actions: {
    // 登录
    Login({
      commit
    }, userInfo) {
      const username = userInfo.username.trim()
      return new Promise((resolve, reject) => {
        login(username, userInfo.password).then(response => {
          const data = response.data
          commit ("SET_USERINFO_DATA",data);
          window.localStorage.setItem('user', JSON.stringify(user.state));
          resolve(response)
        }).catch(error => {
          reject(error)
        })
      })
    },
  }
}

export default user


//getter
const getters={
  userInfo:state=>state.user.userInfo,
}
export default getters

//index
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'
import getters from './getters'

Vue.use(Vuex)

const store = new Vuex.Store({
  modules: {
    user
  },
  getters
})

export default store



```

4. 在页面中调用方法

```javascript
methods:{
  loginSubmit() {
    this.$validator.validateAll("login").then((result) => {
        if (result) {
            this.$store.dispatch('Login', this.userLogin).then(() => {
                this.$router.push({
                    path: this.redirect || '/'
                })
            }).catch(() => {})
          window.localStorage.setItem("userPhone",this.userLogin.username)
        }
    })
  },
}

```


5. 进行路由拦截

```javascript

import router from './router'
import store from './store'


//白名单路由
const whiteList = ['/login', '/forgetPsd', "/"]

router.beforeEach((to, from, next) => {

  //需要登录
  let userInfoId = store.getters.userInfo;
  // console.log(userInfoId)
  //需要登录
  if (userInfoId === null ||userInfoId===''|| typeof userInfoId === 'undefined') {
    if(whiteList.indexOf(to.path)!==-1){
      next()
    }else{
      next(`/login?redirect=${to.path}`)
    }
  }else {
    next()
  }
})

```



