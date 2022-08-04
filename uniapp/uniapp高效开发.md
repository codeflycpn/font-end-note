# uniapp高效开发

## 1 、代码块（tag、js）直接创建组建代码块u+api

`Hbuilder`将uniapp常用的代码封装成一`u`开头的代码块,这些常用的组件必须在components目录下，可以在创建项目的时候选择`uni ui`模板，或者是到插件市场手动把`uniui组件`下载到工程里

例如tag代码块

```vue
<!--输入list-->
<uni-list>
  <uni-list-item title=''note=''></uni-list-item>
  <uni-list-item title=''note=''></uni-list-item>
</uni-list>
```

 例如：js代码块

```js
// 输入ushowtoast
// 得到
uni.showtoast({
  title:'',
  mask:false,
  duration:1500
})
```



## 2、常用js

#### [Tag代码块](https://uniapp.dcloud.io/snippet?id=tag代码块)

- uButton
- uCheckbox
- uGrid：宫格，需引用uni ui
- uList：列表，需引用uni ui
- uListMedia
- uRadio
- uSwiper
- ......

##  [JS代码块](https://uniapp.dcloud.io/snippet?id=js代码块)

##### [uni api代码块](https://uniapp.dcloud.io/snippet?id=uni-api代码块)

- uRequest
- uGetLocation
- uShowToast
- uShowLoading
- uHideLoading
- uShowModal
- uShowActionSheet
- uNavigateTo
- uNavigateBack
- uRedirectTo
- uStartPullDownRefresh
- uStopPullDownRefresh
- uLogin
- uShare
- uPay
- ......

##### [vue js代码块](https://uniapp.dcloud.io/snippet?id=vue-js代码块)

- vImport：导入文件
- ed：export default
- vData：输出 data(){return{}}
- vMethod：输出 methods:{}
- vComponents：输出 components: {}

##### [其他常用js代码块](https://uniapp.dcloud.io/snippet?id=其他常用js代码块)

- iff：简单if
- forr：for循环结构体
- fori：for循环结构体并包含i
- funn：函数
- funa：匿名函数
- rt：return true
- clog：输出："console.log()"
- clogvar：增强的日志输出，可同时把变量的名字打印出来
- varcw：输出："var currentWebview = this.$mp.page.$getAppWebview()"
- ifios：iOS的平台判断
- ifAndroid：Android的平台判断

##### 自定义代码块：详细见文档

## 二、使用HBuilderX内置浏览器调试H5

就是调试、断点、debug、console之类的

