# uniapp框架

## 一、配置

1. pages.json（页面路由）：对uniapp的全局配置 = 小程序app.json
   1. 页面文件路径、窗口样式、原生的导航栏，底部tabbar等
2. manifest.json：应用的配置文件，应用的图标、名称、图标、权限等
3. vue.config是一可选的配置文件，一般用于配置webpack等编译选项

## uni-scss

方便整体控制应用的风格，比如：按钮颜色、边框风格，它是一个特殊的文件，无需import这个文件，即可在scss代码中使用这里的样式变量，uniapp对webpack配置中做了特殊处理，如果想要使用less、stylus全局使用，则需要在vue.config中自行配置webpack策略

* 使用uni-scss：在hbuilder中安装scss插件 +  lang='scss'

  ```VUE
  <style lang='scss'>
  </style>
  ```

* ​	pages.json不支持scss，原生导航栏和tabbar，得使用js-api

## app.vue

`uniapp`主组件，所有的页面都这这里进行切换，是页面的入口文件，但是这里本是不是页面，这里不能编写视图元素

这个文件包括

* `应用的生命周期`周期函数（仅在App.vue中监听，在页面中监听无效）
* 配置全局样式、配置全局的存储globalData
* 不能在这里写template(模板)html代码，只能放置要展示的组件，所以才是单页面

* `globalData`全端通用

  ```vue
  <script>
  	export default {
  		globalData:{
  			text:'这是一条密令'
  		}
  	}
  </script>
  ```

  脚本使用globalData：`getApp().globalData.text = 'test'`

* 状态管理：`vuex`在main中配置定义

* 全局样式：
  * 通用的颜色背景
  * 首页加载动画
  * 多端判断加载对应的样式

## main.js

uniapp的入口文件，`初始化vue实例`、定义全局组件，比如需要状态管理，需要加载插件vuex，具体如下：

```vue
import Vue from 'vue'
import App from './App'
import pageHead from './components/page-head.vue' //全局引用page-head组件

Vue.config.productionTip = false
Vue.component('page-head', pageHead) //全局注册page-head组件，每个页面将可以直接使用该组件
App.mpType = 'app'

const app = new Vue({
    ...App
})
app.$mount() //挂载Vue实例
```

* vue.use：引用插件
* vue.prototype：添加全局变量
* vue.component：注册全局组件

## env（项目环境变量）

* vue-config.js：修改webpack配置
* package.json：自定义编译平台
* .env