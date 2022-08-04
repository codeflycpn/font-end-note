# nvue基本认识

介绍：App端内置了一个基于weex改进的原生渲染引擎，提供了原生渲染能力，在App端，如果使用`vue`页面，则使用`webview`，如果使用nvue页面（native vue 的缩写）则原生渲染

总结：一个App可以同时使用两种也买你



注意点：

* css写法受限，不开发App就可以不用它
* 以往的weex他只是一个高性能的渲染器，没有足够的手机Api能力，比如蓝牙，nvue解决了这个问题，功能很足但是，css写法没有那么飘逸
* `uniapp`手动拓展了weex原生渲染引擎很多的排榜能力



## 使用场景

1. nvue的组件和API写法与vue页面一致，甚至还扩展了一些
2. ·直接去熟练nvue就好

## 纯原生渲染

uniapp在APP端，支持vue和nvue混搭，也支持`纯nvue`渲染

* 开启纯原生渲染：manifest.json源码视图的`"app-plus"`下配置`"renderer":"native"`

  代表App端启用纯原生渲染模式，此时page.json注册的vue页面将会被忽略，vue组将也将被原生渲染引擎来渲染，相对的App端的包体积也会减少，webview渲染模式的相关模块将被移除

* 默认是不开启原生渲染的

## 编译模式

* weex编译模式

  1. 使用`div`

  2. 支持使用weexui，详细见文档

* uniapp编译模式（现在是默认）、

  最新版本已经很吊了

  使用`view`

  如果你选择weex编译模式 -> 将不支持uniapp的租金和jsapi => 参考weex文档来写

* 建议：使用`uniapp`模式，除非weex代码成山了

  | weex编译模式 | uni-app编译模式                   |                                    |
  | ------------ | --------------------------------- | ---------------------------------- |
  | 平台         | 仅App                             | 所有端，包含小程序和H5             |
  | 组件         | weex组件如div                     | uni-app组件如view                  |
  | 生命周期     | 只支持weex生命周期                | 支持所有uni-app生命周期            |
  | JS API       | weex API、uni API、Plus API       | weex API、uni API、Plus API        |
  | 单位         | 750px是屏幕宽度，wx是固定像素单位 | 750rpx是屏幕宽度，px是固定像素单位 |
  | 全局样式     | 手动引入                          | app.vue的样式即为全局样式          |
  | 页面滚动     | 必须给页面套或组件                | 默认支持页面滚动                   |

* weex 编译模式不支持`vuex`
* uniapp辅助weex，原生开发内容不会滚动，uniapp框架会给nvue页面外层添加一个`scroller`实现滚动

## 快速上手

1. 右击新建页面，而不是新建什么nvue文件，页面都需要在page.json中注册，hb编辑器默认注册，其他编辑器不会
2. APP端口默认追寻nvue文件
3. 结构与vue相同：template、style、script
   * template：数据绑定一样，组件支持uniapp和weex
   * style：布局只支持flex，很多高级css写法都不支持
   * script
     * nvue-api：仅支持app端，详见文档
     * uni-api
     * plus-api：仅支持app端
4. nvue调试：hb内置weex调试工具
5. rander-whole：重绘制模式，不关心

## nvue和vue开发的常见区别

1.  隐藏：通过v-if不能使用v-show

2. flex：默认是竖着排列`column`,修改方法如下(不支持其他高级布局)

   ```
   manifest.json` -> `app-plus` -> `nvue` -> `flex-direction
   ```

3. 文字内容只能在`text`组件，text才能设置字体大小颜色
4. 布局不能使用百分比，没有媒体查询
5. nvue切换横屏的时候，可能导致样式出现问题，最好是锁住手机方向
6. 不支持背景图片，可以使用`image`组件
7. nvue组件在安卓端默认是透明的，最号设置bgcolor，否则可能出现问题
8. class绑定只支持数组语法
9. android端使用大量圆角会消耗大量性能
10. 没有回弹效果
11. 默认没有滚动，uniapp默认给你添了一套
12. 在APP-vue中定义全局js变量，不会在nvue中生效，globalData、vuex才可以
13. 不能在style中引入字体文件，详见文档
14. 目前不支持ts
15. ios下拉组件有问题，详见闻文档
