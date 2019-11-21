---
layout: post
title: "VUE去哪儿网学习笔记3"
date: 2018-11-6
tags: [vue]
comments: true
---


进行项目实战环节

# 安装环境
1. 安装node webpack github等环境

2. 全局安装脚手架vue-cli
```
npm install --global vue-cli

```
3. 使用vue-cli构建项目
``` 
vue init webpack travel
```
之后会出现一系列的问题
```
? Project name y
? Project description A Vue.js project
? Author zlstory <13968106594@163.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests No
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recommended) npm
```

# 理论知识
1. 路由
路由就是根据网址不同，返回不同的内容给用户

2. @符号表示src目录下

3. 路由的配置文件放在router文件夹下的index.js中

4. 多页应用：页面之间的跳转，返回的是html，优点是首屏时间快，seo效果好。缺点是：页面之间切换慢。

   单页应用：页面跳转并不是跳转到另一个html，而是通过js删除本页面的dom，加载新的dom。优点是页面切换快，缺点是首屏时间稍慢，seo差。

# 禁用eslint

在webpack.base.conf.js注释代码
```javascript
const createLintingRule = () => ({
  test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: 'pre',
  include: [resolve('src'), resolve('test')],
  options: {
    formatter: require('eslint-friendly-formatter'),
    emitWarning: !config.dev.showEslintErrorsInOverlay
  }
})
```
或者在index.js中：
```javascript
 useEslint: false,
 ```

# 项目实践

1. 改变meta标签，使其适配于移动端
```html
    <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```
2. 使用reset.css清除浏览器默认样式。在assets文件夹中放入静态资源。在main.js中import进去。
```html
import './assets/styles/reset.css'
```
3. 为了解决1像素问题，引入文件：border.css

4. 解决移动端点击延迟问题：引入fastclick.js
   使用`npm install fastclick --save`
   在mian.js中引入并使用
  ```javascript
import fastClick from 'fastclick'
fastClick.attach(document.body)
  ```

5. 在index.js中配置页面路由

  src目录下新建pages文件夹，在index.js中按需引入

6. 在项目中使用stylus：`npm install stylus --save`、`npm install stylus-loader --save`。
    然后在style中定义`lang=stylus`即可
  

7. 在pages/home的文件夹中新建components文件夹，放入Header.vue。是home顶部的组件，然后在Home.vue中使用Header.vue:先引入再注册后使用(注意大小写问题)
```javascript
  <div>
    <home-header></home-header>
  </div>

    import HomeHeader from './components/Header'
     export default {
        name: 'Home',
        components:{
            HomeHeader
        }
    }
```
8. 开发HomeHeader组件。
    使用rem布局：在reset.css中定义html为50px，所以header本来为43px的高度，则为0.86rem

9. 使用iconfont，在main.js中import入iconfont.css(因为多个页面都需要引入iconfont)

10. 为主题颜色写一个公用的css：varibles.styl 为css主题色定义变量，在所需页面中引入即可，方便以后改变主题颜色
```css
/* 在varibles.styl */
$bgColor = #00bcd4
$darkTextColor = #333
$headerHeight = .86rem

/* 在所需页面中先引入再使用 */
<style lang="stylus" scoped>
    @import '../../../assets/styles/varibles'
    .header
        display flex
        height 0.86rem
        line-height 0.86rem
        color #fff
        background $bgColor
</style>
```
由于上面的路径太长了，所以我们可以使用@符号(代表src目录)，需要注意的是在css中引入其他的css，想用@符号时，需在@符号或者别名前面加一个~
```css
 @import '~@/assets/styles/varibles'
```
也可以自定义一个别名来代表assets的styles目录,在webpack.base.conf.js中进行配置
```javascript
 resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      'styles': resolve('src/assets/styles'),
    }
```

11. 使用插件进行首页轮播图的开发：github上搜索：vue-awesome-swiper
先安装再看文档后使用(全局使用)
```
npm install vue-awesome-swiper@2.6.7 --save
```
遇见的坑：在import中引入时出现找不到该模块的声明文件。

12. 新建swiper.vue进行开发

  优化：当网速较慢，图片未加载完成的时候，页面会有抖动。解决办法：swiper外层加一个div.wrapper。再使用css：
  ```
.wrapper
    overflow hidden
    width 100%
    height 0
    padding-bottom 26.67%
or
 .wrapper
    overflow hidden
    height 26.67vw
  ```
  数值是轮播图片的高除以宽得到的

13. 改变swiper的pagination默认样式：>>> 样式穿透  为了不受scoped的限制
```css
.wrapper >>> .swiper-pagination-bullet-active{
    background #ffffff
}
```
14. 为了使icons能够左右滑动，需要借用computed来计算icon是否需要第二页显示。直接在computed中计算就好。
```javascript
  computed: {
      pages() {
          const pages = []
          this.iconList.forEach((item, index) => {
              const page = Math.floor(index / 8) //看数据到底展示在哪一页,从0开始
              if (!pages[page]) { //如果不存在(一开始的情况)
                  pages[page] = []
              }
              pages[page].push(item)//将item放入pages中
          })
          return pages
      }
  }
```
这段代码是为了将data中的iconlists拆分成两个数组。使用vue.js devTools插件可以看得更加清楚
然后在template中循环pages相关内容就完成了。

15. 使用minxin封装一个 内容太多显示...的css
在mixins.styl中
```css
ellipsis()
  overflow: hidden
  white-space: nowrap
  text-overflow: ellipsis
```
使用：先引入该文件 再在所需的样式表中`ellipsis()`即可

16. 在stylus中，通过@import引入的必须是.styl文件，.css文件在index.html中引入，否则报错

17. 开发recommend模板以及weekend模板

18. 由于很多子组件中都需要数据，需要发送ajax请求，而子组件都是显示在home.vue中，所以可以直接在home.vue中发送ajax请求。

将json文件放在static文件夹中，因为在vue-cli生成的所有文件夹中只有static可以被外部(浏览器中输入路径)访问到。

19. 本地将所有模拟数据都是放在mock文件夹中的，所以axios的请求路径是'/static/mock/index.json',但是线上版本应该是'/api/index.json',当我们在本地模拟的时候与上线路径不一致，而在上线前改变代码也是一件非常危险的事情，所以我们需要使用vue代理将请求路径保持一致。

打开config文件夹中的index.js(此功能由webpack-dev-server提供)
```javascript
proxyTable: {
    '/api': {
    target: 'http://localhost:8080',
    pathRewrite: {
        '^/api': '/static/mock'
    }
}
```
更改配置文件之后，需要重新启动服务器。

20. 在home.vue中通过axios得到所有数据，现在需要通过父子组件传值来将数据传到各个子组件中

21. 进行城市选择开发页面，需要配置index.js中的路由信息.

    当我们在头部(home/header.vue)中点击城市选择的时候，会调到city组件，所以在header.vue的header-right部分使用router-link
    
    ```html
    <router-link to="/city">
        <div class="header-right">{{this.city}}<span class="iconfont arrow-icon">&#xe64a;</span></div>
    </router-link>
    ```
 22. 开发city组件时，新建`page/city/city.vue`，然后开发`page/city/components/CityHeader.vue`。(较为简单无技巧)

 23. 开发city中的搜索框界面:当给input给左右内边距时，需要设置
 ```css
  .search-input{
      box-sizing:border-box
  }            
 ```
24. 控制页面上的1像素边框问题
```css
 .border-topbottom
    &:before
        border-color #ccc
    &:after
        border-color #ccc
```

25. 开始列表布局(复杂项)，新建list.vue
(1) 新建三个area，分别是当前城市，热门城市，以A开头的城市，完善布局之后，使用better-scroll

(2) 安装better-scroll

    使用的时候首先要符合bs规定的dom结构(不一定是ul标签)
    
```html
<div class="wrapper">
  <ul class="content">
    <li>...</li>
    <li>...</li>
    ...
  </ul>
  <!-- you can put some other DOMs here, it won't affect the scrolling -->
</div>
```
    然后引入Bscroll,在页面挂载成功之后使用(wrapper是最外层dom的ref)
```javascript
 mounted() {
    this.scroll = new BScroll(this.$refs.wrapper)
}
```
简单的滚动完成

26. 开发左边字母表组件(alphabet.vue)

27. 动态渲染city组件啦！
在city.vue组件中，使用axios.get获取城市列表，然后父子组件传值到city-list组件中。

循环中再循环了解一下:
```html
<div class="area" v-for="(item,key) of cities" :key="key">
    <div class="title border-topbottom">{{key}}</div>
    <div class="item-list">
        <div class="item border-bottom" v-for="innerItem of item" :key="innerItem.id">
            {{innerItem.name}}
        </div> 
    </div>
</div>
```
28. 兄弟组件联动：点击右边字母表，左边滑动到对应的位置

    思路：将alphabet.vue中的字母通过点击事件拿到对应的innerText，将其传值给兄弟组件list.vue(先将alphabet.vue中的值传递给父组件city.vue,再将数据从city.vue中传递给list.vue)

    alphabet.vue:在字母表的item中绑定一个点击事件,向外触发事件
    ```java
     handleLetterClick(e){
        this.$emit('change',e.target.innerText)
    }
    ```
    city.vue：监听子组件传递过来的change事件
    ```html
     <city-alphabet :cities="cities" @change="handleLetterChange"></city-alphabet>
    ```
    然后在methods中定义`handleLetterChange`事件,然后将letter传值给list.vue
    ```javascript
     handleLetterChange(letter) {
            //拿到alphabet.vue中的字母值。
            console.log(letter)
            this.letter = letter
        }
    ```
    当list.vue拿到点击的letter值时，思路：当letter改变时，我们需要找到对应字母的列表。
    ```javascript
    props: {
        letter: String
    }
    watch: {
        letter() {
            console.log(this.letter)
        }
    }
    ```
    然后通过调用better-scroll提供的`scrollToElement()`方法控制左边列表滚动到对应的列表上面
    ```html
    <div class="area" v-for="(item,key) of cities" :key="key" :ref="key">
        <div class="title border-topbottom">{{key}}</div>
        <div class="item-list">
            <div class="item border-bottom" v-for="innerItem of item" :key="innerItem.id">
                {{innerItem.name}}
            </div>
        </div>
    </div>
    ```
    ```javascript
    watch: {
        letter() {
            if (this.letter) {
                const element = this.$refs[this.letter][0]
                this.scroll.scrollToElement(element,400)
            }
        }
    }
    ```
29. 右侧字母表监听滚动事件
    思路:获得字母A到顶部的距离，当滑动的时候获取手指距离顶部的高度，得到差值之后除以字母之间的高度。这样就知道当前是第几个字母了。

    ```java
    handleTouchMove(e) {
        if (this.touchStatus) {
            const startY = this.$refs['A'][0].offsetTop
            const touchY = e.touches[0].clientY - 79
            const index = Math.floor((touchY - startY) / 20)
            if (index >= 0 && index < this.letters.length) {
                this.$emit('change', this.letters[index])
            }
        }
    }
    ```
    上述写法是比较耗性能的，因为offsetTop一直在改变。所以使用updated钩子函数
    ```javascript
     updated() {
            this.startY = this.$refs['A'][0].offsetTop
        },
    ```
    再进行函数节流：延迟16毫秒去执行
```javascript
handleTouchMove(e) {
    if (this.touchStatus) {
        if (this.timer) {
            clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
            const touchY = e.touches[0].clientY - 79
            const index = Math.floor((touchY - this.startY) / 20)
            if (index >= 0 && index < this.letters.length) {
                this.$emit('change', this.letters[index])
            }
        }, 16)
    }
},
```
30. 在城市选择的时候，点击搜索框，根据用户输入信息实时显示所匹配的信息。

    首先通过父子组件之间传值获取到所有cities的数据，然后通过v-model的双向数据绑定去获取到用户的值，再与cities中的数据进行匹配

```javascript
const result = []
for (let i in this.cities) {
    this.cities[i].forEach(value => {
        if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
            result.push(value)
        }
    });
}
this.list = result
```

31. 熟悉vuex

    因为city.vue与home.vue之间是没有共同父组件的，如果要进行两个组件之间的通信的话，可以使用vue官方推荐的vuex(数据框架)

    为什么需要vuex(设计理念)：

    当我们的项目中有多个页面或者是多个组件之间进行复杂的数据传值很困难的时候，可以将公用的数据放在公共的存储空间去存储，当我们在某一个组件中改变数据的时候，其他组件就可以感知到数据的变化。

    图解：
    vuex由哪几部分组成：

    state：存储公用数据，需要公用数据的时候，直接调用state就好了
        当我们需要改变数据的时候,我们不能用组件(Vue components)直接改变数据，需要走一个流程：
            (1)如果有异步操作、复杂的同步操作、批量的同步操作，我们将异步操作放在actions中，
            Vue components -> Actions -> Mutations -> Satate
            或者 Vue components -> Mutations -> Satate

32. 在项目中使用vuex
本应该在main.js中引入vuex的，但是为了更方便管理，我们将其放在新的位置
在src目录下新建store文件夹，在该文件夹下新建index.js
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    city: '北京'  //在city组件和home组件中关联起来的一个值
  }
})
```
然后在main.js中`import store from './store'`
并在根组件中注册store

33. 注册store之后，现在开始使用store中的数据

home.vue中以前home-header组件中的city值是由外部传入进去的，现在我们并不需要后端传值给我们了，而是由前端存储的，所以删掉`:city = "city"`以及data中的city默认值

header.vue中之前关于接收city的props也可以删掉了，而使用`{{this.$store.state.city}}`拿到我们刚刚在store中存储的值

34. 改变state：在城市选择的列表里，点击哪个城市，state值就为哪个城市。

    在list.vue中给热门城市的item绑定一个handleCityClick事件，使用dispatch方法改变actions
```javascript
 methods: {
    handleCityClick(city) {
        this.$store.dispatch('changeCity', city)
    }
},
```
又因为改变的actions，所以我们需要在刚刚的store/index中添加一个actions，并接收两个参数
```javascript
export default new Vuex.Store({
  state: {
    city: '杭州'
  },
  actions: {
    changeCity(ctx, city){
        console.log(city)
    }
  }
})
```
actions又通过commit方法来调用mutations去改变公共数据，所以又需要新建一个mutations
```javascript
export default new Vuex.Store({
  state: {
    city: '杭州'
  },
  actions: {
    changeCity(ctx, city){
        ctx.commit('changeCity',city)
    }
  },
  mutations:{
    changeCity(state,city){
        state.city = city
    }
  }
})
```
正常流程是以上步骤，但是由于本项目开发是没有异步数据也没有批量处理同步数据，所以我们可以不走actions这一步，那么做出的改变就是
```javascript
//list.vue
 methods: {
    handleCityClick(city) {
        this.$store.commit('changeCity', city)
    }
}

//index.js
export default new Vuex.Store({
  state: {
    city: '杭州'
  },
  mutations:{
    changeCity(state,city){
        state.city = city
    }
  }
})
```

然后在所需点击地方加入对应的方法即可。

35. 在点击对应的城市完毕时，返回首页内容(vue-router)，使用router.push
```javascript
 handleCityClick(city) {
    this.$store.commit('changeCity', city)
    this.$router.push('/')
}
```

36. vuex的高级使用以及localstorage

把组件的共享状态抽取出来，以一个全局单例模式管理


当我们选择城市时，在进行一次刷新操作，页面又变成了默认的城市，所以我们需要用localstorage去存储数据，这样子我们下一次进入该网站时也是上一次选择的城市。

需要改变index.js中的内容(比较简单),直接使用localstorage就可以了
```javascript
export default new Vuex.Store({
  state: {
    city: localStorage.city || '杭州'
  },
  mutations: {
    changeCity(state, city) {
      state.city = city
      localStorage.city = city
    }
  }
})
```
当某一些用户使用隐身模式或者禁用了本地存储功能，为了使代码正常运行，我们需要在所有localstorage外层包裹一层try catch
```javascript
let defaultCity = '上海'
try {
  if (localStorage.city) {
    defaultCity = localStorage.city
  }
} catch (e) {}

export default new Vuex.Store({
  state: {
    city: defaultCity
  },
  mutations: {
    changeCity(state, city) {
      state.city = city
      try {
        localStorage.city = city
      }catch(e){}
    }
  }
})

```
在实际项目中，为了使代码更加规范易懂以及更方便维护，所以我们会再建立state.js与mutations.js,然后将index.js代码拆分出来
```javascript
//state.js
let defaultCity = '上海'
try {
  if (localStorage.city) {
    defaultCity = localStorage.city
  }
} catch (e) {}

export default {
    city: defaultCity
  }

//mutations.js
export default {
  changeCity(state, city) {
    state.city = city
    try {
      localStorage.city = city
    } catch (e) {}
  }
}

//index.js
import Vue from 'vue'
import Vuex from 'vuex'
import state from './state'
import mutations from './mutations'

Vue.use(Vuex)

export default new Vuex.Store({
  state,
  mutations
})

```
37. mapState
由于页面中的`this.$store.state.city`太长了，所以我们可以使用vuex提供的api`mapState`
```javascript
import {mapState} from 'vuex'
export default {
    name: 'HomeHeader',
    computed:{
        ...mapState(['city'])
    }
}
```
这样在页面中使用`{{this.city}}`即可

同理{mapMutations}也是先引入再使用，使代码看起来更加简洁。

38. 使用keep-alive优化性能
由于切换路由的时候，组件都会被重新渲染，导致mouted()钩子会重新执行，所以每一次都会发送请求。

但是项目中的json数据并无改变，所以我们可以在app.vue中使用keep-alive

使用keep-alive之后，monted()不会执行，但是actived()会执行，所以需要处理什么，可以放在actived钩子函数之中

actived():当页面重新被显示的时候执行


39. 开发详情页

在recommend.vue中使用router-link标签来进行页面跳转并进行参数的传递
```html
<router-link :to="'/detail' + item.id" tag='li'>
   ...
</router-link>

```

40. 在src目录下新建common文件夹，放入的是公用的组件,并在webpack.base.config.js中为此目录创建一个别名

41. 在vue开发时，需要注意的是解绑全局事件。如果是对某个标签的事件进行绑定，那么将不会造成影响。但是如果事件是window事件的话将会对其他页面造成一定的影响，所以我们需要进行全局事件的解绑
当在组件中使用keep-alive时，此组件会多出一个actived()钩子函数，在每次页面展示的时候会执行。与之对应的会有另一个生命钩子函数叫做deactivated(),在页面即将被隐藏的时候执行。
```javascript
 activated() {
    window.addEventListener('scroll', this.handleScroll)
},
deactivated() {
    window.removeEventListener('scroll', this.handleScroll)
}
```
42. 使用递归组件：在组件自身调用组件自身
数据格式
```javascript
list: [{
        title: "成人票",
        children:[{
            title:"成人三馆联票",
            children:[{
                title:"成人三馆联票-某连锁销售"
            }]
        },{
            title:"成人五馆联票"
        }]
    }, {
        title: "学生票"
    }, {
        title: "特惠票"
    }, {
        title: "儿童票"
    }]
}

```

```html
<div>
    <div class="item" v-for="(item,index) of list" :key="index">
        <div class="item-title border-bottom">
            <span class="item-title-icon"></span>
            {{item.title}}
        </div>
        <div v-if="item.children" class="item-children">
            <detail-list :list="item.children"></detail-list>
        </div>

    </div>
</div>
```
43. 通过不同id值传参给后端

在index.js中设置动态路由的时候，会将动态的id以参数的形式设置好
```javascript
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home
    },
    {
      path: '/city',
      name: 'City',
      component: City
    },
    {
      path: '/detail/:id',
      name: 'Detail',
      component: Detail
    }
  ]
})

```

在发送请求的时候

传统方式是通过字符串拼接：
```javascript
  getDetailInfo(){
    axios.get('/api/detail.json?id='+this.$route.params.id)
}
```
更为直观的传参方式

```javascript
 getDetailInfo() {
    axios.get('/api/detail.json', {
            params: {
                id: this.$route.params.id
            }
        })
    }

```




