---
layout: post
title: "使用element-ui搭建后台管理系统问题汇总"
date: 2019-2-20
tags: [vue]
comments: true
---

#### 时间插件

1. 两个输入框，限制结束日期不能大于开始日期

注意此时input框中不能使用value-format，这会使日期变成字符串从而不能比较大小
```javascript
<el-date-picker
    type="date"
    placeholder="开始日期"
    v-model="dayStartTime"
    :picker-options="pickerOptions0">
</el-date-picker>
<el-date-picker
    type="date"
    placeholder="结束日期"
    v-model="dayEndTime"
    :picker-options="pickerOptions1">
</el-date-picker>


 data() {
      return {
        pickerOptions0: {
          disabledDate: (time) => {
            if (this.dayEndTime != "") {
              return time.getTime() > Date.now() || time.getTime() > this.dayEndTime;
            } else {
              return time.getTime() > Date.now();
            }
          }
        },
        pickerOptions1: {
          disabledDate: (time) => {
            return time.getTime() < this.dayStartTime || time.getTime() > Date.now();
          }
        },
        dayStartTime: '',
        dayEndTime:'',
      }
    }

```

其他的时间限制：[更多限制](http://www.cnblogs.com/xjcjcsy/p/7977966.html)

2. 获取到的日期与选中的日期相隔一天

一般情况下使用value-format即可解决这个问题

```javascript
<el-date-picker
    type="date"
    placeholder="开始日期"
    v-model="dayStartTime"
    value-format="yyyy-MM-dd"
    :picker-options="pickerOptions0">
</el-date-picker>
```

但是在比较大小的时候不能使用value-format，所以使用change事件获取到值
```javascript
 <el-date-picker
    type="date"
    placeholder="开始日期"
    v-model="dayStartTime"
    @change="getStartTime"
    :picker-options="pickerOptions0">
</el-date-picker>


methods: {
      getStartTime(time){
        this.dayStartTime = time
      }
    }

```

#### 通过id跳转到不同的详情页

```javascript
created() {
    const dataID=this.$route.query.id
    request({
        method:'get',
        url:`/web/admin/downstream/merchantList/${dataID}`
    }).then((res)=>{
        this.list = res.data
    })
}
```

####  背景图片打包后找不到路径问题

```javascript
<style lang="stylus" scoped>
.bg
  background url("~@/assets/psdBg.png") no-repeat
  background-size 100% 100%
</style>

// 在build/utils.js
   if (options.extract) {
      return ExtractTextPlugin.extract({
        publicPath:'../../',   //添加此行
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }

```

#### Table




