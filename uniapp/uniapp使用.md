# uniapp必知



## 一、开发规范

1. `开发约定`
   + `页面`文件遵循 `Vue文件组件（SCF）规范`
   + `组件`遵循小程序规范
   + `接口`(JS API)靠近小程序规范，将前缀`wx`换成`uni`
   + `数据`+`事件`=> `Vue.js` 规范   ：同时补充了App及页面的生命周期
   + 为了兼容多端：`尽量使用flex布局`

## 二、目录结构

```
┌─uniCloud              云空间目录，阿里云为uniCloud-aliyun,腾讯云为uniCloud-tcb（详见uniCloud）
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─uni_modules           存放[uni_module](/uni_modules)规范的插件。
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，详见
├─pages.json            配置页面路由、导航条、选项卡等页面类信息，详见
└─uni.scss              这里是uni-app内置的常用样式变量 
```

`注意`

1. 编译到任何平台，
   * `static`包都会被打进去，但不会编译
   * `static`目录下js文件不会被编译：如果里面有es6代码，会因为没经过转换而报错
   * `css、less、scss`资源不宜放置`static`目录下，建议放置`common`目录下
   * 有效目录的概念

## 三、资源引入

模板内、js文件同样适用，以下两种方式引入文件

* 绝对路径：`/static/logo.png`（js文件不支持）   ||  `@/static/logo.png`
* 相对路径：`../../static/logo.png`

css引入静态资源=> css文件，（scss、less文件同理）可以使用相对路径和绝对路径

```css
/*绝对路径*/
@import url('/common/uni.css')
@import url('@/common/uni.css')
/*相对路径*/
@impost url('../../common/uni.css')
```

iconfont 字体图标，导入的时候选择base64加持，

## 四、生命周期

* 应用生命周期：vue +  各种小程序的生命周期
* 页面生命周期：vue +  各种小程序的生命周期

## 五、路由跳转

跳转方式：

1. `navigator`组件包裹，跳转
2. 铜鼓调用api跳转

路由存在形式：`页面栈结构`就是数据结构里的栈=>`后进先出`只有一个口

请看以下结构：注意理解

`页面初始化（有页面入栈）=>我又打开一个页面（有页面入栈）=>页面重定向（当前页出栈，新页面入栈）`

=>返回（页面出栈）=>Tab切换（全部页面出栈，只留下新的tab页面）=>重加载（全部页面出栈，只留下最新的页面）

## 六、运行环境判断

* 开发环境
* 生产环境

`uni-app`  => `process.env.NODE_ENV`：用于连接测试服务器和生产服务器的动态切换

* 通过`运行`编译出来的代码都是：开发环境

* 通过`发行`编译出来的代码都是：生产环境

* cli模式是通过编译环境处理的

  ```js
  if(process.env.NODE_ENV === 'development') {
      console.log(‘开发环境’)
  }else{
  	console.log('生产环境')
  }
  ```

* 自定义更多环境`https://uniapp.dcloud.io/collocation/vue-config`

* 判断环境代码快捷键

  ```js
  // 输入 uEnvDev
  
  if (process.env.NODE_ENV === 'development') {
      // TODO(代办事项)
  }
  // 输入uenvProd
  
  if (process.env.NODE_ENV === 'production') {
      // TODO
  }
  ```

## 七、平台判断

判断平台有两种场景：编译期、运行期

* 编译期（条件编译）：通过总平台编译，各个平台拿到的包都是不一样的

  ```js
  // #ifdef H5
   alert('只有h5平台才支持alert方法')
  // #endif
  ```

* 运行期： 是我们在运行的时候，有安卓和ios、其他平台，他们之间也是有区别的，这时候我们也要做个处理

  通过：uni.getSysteminfoSync().platform=>判断客户端（android、ios，小程序开发工具、等！）

  ```js
  
  export function getUseOfTool(){
      switch(uni.getSystemInfoSync().platform){
      case 'android':
         console.log('运行Android上')
         break;
      case 'ios':
         console.log('运行iOS上')
         break;
      default:
         console.log('运行在开发者工具上')
         break;
   }
  }
  ```

* 还有一些其他的环境变量

## 八、页面样式与布局

1. 页面种类

   1. vue页面：通过webview渲染
   2. nvue页面：app端的nvue是原生渲染：在样式方面会比web限制很多。详见`https://uniapp.dcloud.io/nvue-css`

2. 尺寸单位

   支持通过的css单位：rpx、px

   px：屏幕像素

   rpx：响应式的px：357的宽度就正好是1:2

   计算配置参考文档

   注意：vue页面是支持h5单位但在nvue里不支持

   * rem字根大小可以通过page—meta配置
   * vh（viewpoint   height）：视窗高度，1vh = 视窗高度1%
   * vw（viewpoint   width）：视图宽度，1vw = 视窗宽度1%

3. nvue不支持百分比单位

4. nvue的两个模式

   1. weex（老模式）：同weex标准写法，只能在app端运行，部分uni-app js Api 不能使用

   2. 开启`dynamicRpx` 后可以适配屏幕大小动态变化

   3. uni-app（新模式 && 默认模式）：支持的更多

      解释：我们要把小程序的所有组件，在weex的原生渲染机制上，重新实现一遍，让小程序的组件可以直接用原生来渲染。然后包括uni ui等扩展组件也都将兼容nvue

   4. px：

   5. wx：

5. rpx的详细说明

   1. uniapp在App端+h5都支持`rpx`

   2. uniapp规定基准的一个宽度为750rpx=>对标375px宽度的手机

   		  3. `计算公式` =  750 * 元素在设计稿中的宽度 / 设计稿基准宽度

          ```
          若设计稿宽度为 750px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 750，结果为：100rpx。
          若设计稿宽度为 640px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 640，结果为：117rpx。
          若设计稿宽度为 375px，元素 B 在设计稿上的宽度为 200px，那么元素 B 在 uni-app 里面的宽度应该设为：750 * 200 / 375，结果为：400rpx。
          ```

   		  4. 注意点：
        	
        		  1. rpx是动态的宽度单位，屏幕越宽该值的实际像素越大，如果不希望缩放，清使用px单位
        		  2. 早期uni-app提供upx，现在统一该为rpx了
        		  3. 一般情况下高度和字体大小是不应该根据屏幕宽度（等比）变化的。
        		  4. rpx不支持动态横竖屏切换计算，使用rpx建议锁定屏幕方向
        		  5. rpx换算：https://ask.dcloud.net.cn/article/35445。
        		  6. App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px，不支持 rp

## 九、样式

1. 样式导入：@import  +  相对路径和绝对路径

2. 尽量将代码写到class里面，style在接受动态样式时会进行动态解析同时就会影响渲染速度

   ```html
   <view :style="{color:color}">
   ```

3. class：用于指定样式规则

   ```html
   <view class='normal_view'>
   ```

4. 支持的选择器，详见官方文档

5. page相当于body节点

   ```css
   <!-- 设置页面背景颜色，使用scoped会导致失效>
   page{
       background-colro：#ccc
   }
   ```

6. 全局样式与局部样式

   * `定义在App.vue`中的样式都是全局样式  => 作用 与每一个页面
   * 在pages目录下的vue文件中定义的文件样式都为局部样式，并会覆盖App.vue中相同的选择器
   * 重点：
     * App.vue中通过`@import`语句导入外联样式，这样就作用于每一个页面——这是一个前段工程化的思路
     * nvue页面暂不支持全局样式（没试过）

7. css变量

   uni-app提供了内置的css变量

   * --status-bar-height :系统状态栏高度
   * --window-top：内容区距离顶部的距离 = navigationBar的高度
   * --window-bottom，内容区距离底部的距离 = Tabbar的高度
   * 注意点：详见文档

8. 代码块

9. 固定值

   `uniapp`以下几个组件的高度是固定的，不可修改

| 组件          | 描述       | App                                                          | H5   |
| :------------ | :--------- | :----------------------------------------------------------- | :--- |
| NavigationBar | 导航栏     | 44px                                                         | 44px |
| TabBar        | 底部选项卡 | HBuilderX 2.3.4之前为56px，2.3.4起和H5调为一致，统一为 50px。但可以自主更改高度） | 50px |

各小程序平台，包括同小程序平台的iOS和Android的高度也不一样

10. 为了更好的跨端，我们统一使用Flex布局

11. 背景图片

    `uniapp`支持使用在css里设置背景图片，使用方式与`web`大体相同

    * 支持base64格式图片

    * 支持网络路径图片

    * 小程序不支持使用本地文件，包括本地的背景图片字体文件，需要以base64方式使用

    * 使用本地路径背景图片需要注意

      * 为了方便开发者，以图片的大小40kb作为一个分水岭
        * 小于40kb，uni编译到不支持本地背景图的平台时，会自动将其转化为base64格式
        * 大于40kb会有性能问题，不建议使用，需要手动的将其转为base64，将其挪到服务器上，使用网络地址使用

    * 本地背景图片的引用路径推荐使用以@开头的绝对路径

      ```css
      .test2{
        background-image:url('~@/static/logo.png')
      }
      ```

      小程序不支持相对路径

12. 字体图标

    uni使用字体图标基本与web相同，注意以下几点

    * 支持base64格式字体

    * 支持网络字体

    * 小程序不支持在css中使用本地文件，包括本地背景图，和字体文件，需要以base64呈现

    * 网络路径必须以`https`的协议头 ，默认是不加的

    * 注意40kb的分水岭

    * 字体文件的引用

      ```css
      @font-face{
        font-family:test1-icon;
        src:url('~@/static/iconfont.ttf')
      ```

    * ​		nvue中引入iconfont的方式(weex官网中有)

      ```js
      var domModule = weex.requireModule('dom');
      domModule.addRule('fontFace', {
        'fontFamily': "fontFamilyName",
        'src': "url('https://...')"
      })
      ```

      示例

      ```vue
      <template>
          <view>
              <view>
                  <text class="test">&#xe600;</text>
                  <text class="test">&#xe687;</text>
                  <text class="test">&#xe60b;</text>
              </view>
          </view>
      </template>
      <style>
          @font-face {
              font-family: 'iconfont';
              src: url('https://at.alicdn.com/t/font_865816_17gjspmmrkti.ttf') format('truetype');
          }
          .test {
              font-family: iconfont;
              margin-left: 20rpx;
          }
      </style>
      ```

13. <template/>和<block>

    `uniapp`支持在template模板中套用`<template>`和`<block>`,进行`列表渲染`和`条件渲染`

    1. 他们并不是一个组件，他们是无形的包装器，不做页面渲染，只接受控制属性

    2. <block/>在不同的平台有差异，推荐使用<template>

    3. 演示

       ```vue
       
       <template>
           <view>
               <template v-if="test">
                   <view>test 为 true 时显示</view>
               </template>
               <template v-else>
                   <view>test 为 false 时显示</view>
               </template>
           </view>
       </template>
       ```

       ```vue
       <template>
           <view>
               <block v-for="(item,index) in list" :key="index">
                   <view>{{item}} - {{index}}</view>
               </block>
           </view>
       </template>
       ```

       

14. ES6支持

    uniapp支持大部分的ES6API的同时，也支持ES7的await/async（有一些还是不支持的，遇到了也不要怀疑自己的代码写错樂）

15. npm支持（这里有文档需要查看）=> 为了多端考虑，优先使用插件市场

    * 你非h5端的肯定不支持浏览器的api什么dom，alert之类的
    * node_modules,必须在根目录下
    * ui库的获取详见官网

16. TypeScript支持

    * 学会ts的时候再看吧

17. 小程序自定义组件支持（注意文件位置和格式详细见官网）

18. wxs使用时看官网

19. renderjs

    * 只支持app-vue和h5

    



