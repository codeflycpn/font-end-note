## vue

> 1.基础篇
>
> * vue是什么
> * 数据绑定
> * 计算属性
> * bind动态绑定数据和样式
> * 内置指令
> * 表单和v-model
> * 组件详解
> * 自定义指令
>
> 2.进阶篇
>
> 3.实战开发

## 一、vue是什么

1.1：vue介绍

核心简单小巧，渐进式技术栈，但足矣应对任何规模的应用

小巧：vue压缩后只有17k

渐进式：一步一步的有阶段性的构建你的项目，并不需要一开始就需要用大量的内核                                   

颠覆：

* 解耦识图和数据
* 组件化开发
* 前端路由
* 状态管理
* 虚拟Dom

1.2：mvvm

MVVM：MVC演变过来的

1. Model-View|View-Model
2. 视图层和数据模型层相互影响

1.3：vue的突破

Vue在jQuery上的突破，对比如下，一个死报dom，一个只需关注数据模型，直接操作dom经过项目的重重迭代，后续将非常难以维护

```js
// jQuery操作添加一个数据
$('#app').append('dom')

// vue操作添加一个数据
this.arr.push({})
```

1.4：vue的开发模式

使用vue的两种方式：cdn引入，下载vue包

```html
<script src='https:// unpkg.com/vue/dist/vue.min.js/>
             
<script>
         new Vue({
            el:'#app',
            data:{
                  books:[
                     	{name:'vue.js'},
                      {name:'语言精粹'},
                      {name:'高级程序设计'}               
                       ]                           
                 }                   
             })    
             
</script>
```

这只是一个非常简单的vue应用，如果要构建非常健硕的企业级应用，你会用到很多拓展性的东西，比如：css预处理Es6转Es5等这些vue并不负责处理，这时候就可以配合`webpack`这个打包工具使用，可以使用更高级的`vue-router`进行路由管理，使用`vuex`进行页面的状态管理

## 二、vue数据绑定渲染

2.1：实例与数据

```js
// 通过构造函数创建实例，并启动vue应用
let app = new Vue({
  // 选项: 所有的js代码都会被注入到选项内
})


```

实际上这个单页面应用程序就是这个`app`

2.1.2：vue选项

*  el：将vue挂载到某个dom上

  ```js
  let vue new Vue({
    	el:domcument.getElementById('app')
  })
  ```

  1. 我们可以通过app.$el来访问该元素

* data：模型中的数据，用于绑定在视图中的数据，为了更好的维护项目，应该将视图中所需要用到的数据都在data中声明，不至于散落在业务逻辑中

  ```vue
  <div>{{name}}</div>  
  
  vue({
  	data:{
  		name:'我的发'
  	}
  })
  ```

  vue实例本身也是代理了data对象中所有属性

  ```js
  vue({
    data{
    	name:'我的发'
  	}
  })
  console.log(app.nname) // 我的发
  ```

* 2.1.3声明周期

  Vue实例创建到销毁会经历一系列的状态，一个vue就是当前页，

  我们可以在不同的状态进行恰当的操作，比如在onshow中检查是否需要刷新页面

> 1. created:实例刚创建完，已经有这个东西了，但是还没挂载dom，我们可以在这里操作与dom无关的操作，比如获取异步数据，获取参数等
>
> 2. mounted：el挂载到实例后才调用，初始化的dom操作可以放在这里
> 3. beforeDestoy：实例临时前要做的事情，比如解绑addeventListener监听事件等
> 4. .... 更多请查看文档

` 这些钩子和el一样作为选项写在vue实例内 `

2.1.4插值与表达式：Mustache语法=> {{ data里的属性 }}

​		 插值增强版{{ 这里的数据可以被函数加工 }}

​		 如果碰巧只想渲染{{}}，通过指令v-pre即可完成

​		 {{  }} + 简单运算，不能进行复杂运算

```js
{{ number / 10 }}  // 基本运算
{{ isOk?'确定':'取消' }} // 三目运算
{{ text.split(',').reverse().join(',') }} // 字符串操作
```

2.1.4 过滤器

用于格式化文本，比如格式化时间，在{{ data |  formdate  }} 通过 | 对数据进行过滤

使用：在vue对象中的选项中添加filter属性

```vue
{
	filters:{
			// ...多个过滤器
			// value 是管道前面的数据，也就是需要过滤的数据
			formatdate(value){ return  value +1  }
  }
}
```

  也可以接受多个参数

{{ data | filterA(arg1,arg2)  }}   这里的arg1，2是对应第二第三个参数

`过滤器适用于处理简单的文本，复杂的使用计算是属性`



2.2.指令与事件

Directives，以`v-`为前缀：当表达式的值发生改变时指令做出它的作用

* v-if
* v-html
* v-pre

例如：当show = false时，移除view，当show=ture时添加view => 这就是数据驱动dom，视为vue的核心理念

，所以我通常通过数据操作dom，并不直接操作dom，我们只需要维护好数据即可

`v-bind`动态驱动元素属性

例如：v-bind:src='link'  // 那么就可以通过变量来动态传入src了

`v-on`绑定事件监听器在元素上，百分之80的交互都在这了产生

v-on:click | kyeup | mousemove等监听多种事件

两种写法

* 外联：将=右边的表达式配置为一个函数名，`推荐`

  ```vue
  <bottom v-on:click='send'/>
  
  {
  	method:{
  		send(){
  				// 业务逻辑
  				// 函数内的this指向是vue本身，因此可以直接使用，this.xx访问和修改数据
       }
    }
  
  }
  ```

  

* 内联：简单的表达式，修改个值啥的

  ```vue
  <bottom  v-on:click='show=!show' />
  ```

* vue已经将method里的方法代理和data一样，所以可以通过this.function来调用任何函数

  ```vue
  let app = new Vue({})
  methods:{
  	func1(){
  		this.func2()
    },
  	func2(){}
  }
  // 甚至可以在vue外部调用
  app.func1()
  ```

  2.3 语法糖

  所谓语法糖指的是，在不影响功能的情况下，将语法简化后还能达到同样的效果，从而方便开发，比如v-bind=>`:`  :src='link'，还有其他

## 三、内置指令





## 三、计算属性

处理复杂的模板数据、class、style、动态传输props的数据处理，更好的维护

```vue
computed:{
		reverseText(){ return ....}
}
```

1. 用法

   * 可以进行复杂逻辑运算和调用this函数

   * 不可以接受参数

   * 需要返回一个结果

   * 可以依赖多个data属性，当其中一个值变化的时候，就会触发计算属性，视图再次更新

   * 计算属性的setter和getter

     一个是被动输出另一个是主动改变,getter：当依赖的值发生变化时被动触发刷新视图，setter是该计算属性发生变化时，才会触发

2. 细节：

   * 计算属性之间可以相互依赖
   * 可以依赖其他实例的数据

3. 计算属性缓存问题

   当该计算所依赖的值发生改变后就会立即触发计算属性，但是methods里的方法就不会了

## 四、动态样式

页面开发中，经常要动态的处理样式，主要通过·`v-bind`指令来配合

### class

* 对象语法

  ```vue
  <div :class="{'textred': isColor,'eerror,'...' }" />
  ```

  ·当情况较为复杂时，可以使用计算属性

  ```vue
  <div :class='setClass'/>
  
  computed:{
  	// 返回一个对象正好和class的格式需求对应上了
  	setClass(){
  		// 用计算属性的话说就至少两个条件起步了
  		return  { active:this.isActive && !this.showText,‘text-fail’:this.err    } 
    }
  }
  ```

  或则绑定一个类对象

  ```vue
  <div :class='setClass'/>
  
  data(){
  		return {
  				setClas:{color:false,hight:true}
  		}
  }
  ```

* 数组语法

* 在组件上使用动态样式

  如果直接上自定义组件上使用class、:class，根据样式规则，会直接作用在根元素上

### style

用法和class的类似，对象语法和计算属性比较常用

* 对象语法

  css使用驼峰命名或者-命名

  ```vue
  <div :style="{'color':color,'marginLeft':size+'rpx'}"  />
  ```

* 加强版写法，`写在对象里或者computed`

  ```vue
  <div :style='style' />
  
  data:{
  	style:{color:'red',fontsize:size+'rpx'}
  }
  computed :{
  		style(){
  			return `color:'red'`
     }
  }
  ```

* 应用多个样式对象，可以使用数组语法

  ```vue
  <div :style="[styleA,styleB]"    />
  ```

* 注意：使用:style时vue会自动给特殊css属性名称增加前缀，比如，transform



## 五、指令

### 冷门指令

v-cloak：在vue实例结束编译时，从dom移除，经常与display：none使用

情景一：网速很慢：vue上的{{message}}还没被解析，就会直接漏出来，过后vue将会解析{{}}

但是屏幕将会闪动一下，只要加上一句[v-cloak]{display :none}即可

v-once：定义它的元素在整个vue生命周期中只会触发一次，不会随着data的改变而改变

### 条件渲染指令



## 七、组件详解

组件是vue的核心功能，也是整个框架设计最精彩的地方

### 7.1为什么要使用组件

一般在前端工程化开发中，我们都会在多处使用一个布局组件，复制粘贴是憨p行为，为什么呢？如果在组件中有一个点击事件，那么要现实它，我们需要在多个页面都得实现这个方法，那么既然他们的点击效果是一样的，那么，我们直接委托给子组件就可以了，本页面不需要写，直接让子组件替我们实现

最后组件最伟大的在于后期的维护，基本上只需要修改组件上的内容即可，一呼百应

### 7.2使用用法

组件需要注册后才能使用，分为`全局注册`和`局部注册`

推荐使用减号命名

在父组件使用组价，必须是先注册，

```vue
<div id="app">
  <my-component></my-component> // 等同于 '<div>这里是组件的内容</div>
</div>

<script>
  
var app = new Vue({
  el:'#app
  // 局部注册组件
  component:{
   		'my-component':Child
	}
})

// 全局注册组件
Vue.component('my-component',{
  // 选项
  template：'<div>这里是组件的内容</div>
})

</script>
```

在vue2版本中template的Dom结构必须被一个元素包裹着，如果直接写成这里面是组件的内容，不带div标签，这就不能正确渲染了

组件除了template选项外，还可以项vue实例的那些选项，例如，data，mehtods、component等，组件也是可以嵌套组件的

`关于data为什么是个函数`

```vue
let data = {
		name:'林发挥'
}

Vue.component(’my-component’, {

   template:’<button @click=”counter++”>{{ counter ))</button>’, 
   data:function () {
      	return data ; 这里返回的是一个已经在内存里对象，这里是返回它的地址
     }
}) 

```

js的对象是引用关系，也就是会存在深拷贝和浅拷贝的问题，如果函数只是把这个对象return出去，那你们得到的也只是它的指针而已

所以我们应该直接返回一个新的值，已不是已有的对象

```vue
data(){
		return {
			name:'林发挥'
    }
}
// 以下三个页面都用到该组件，后续再研究

页面A  
		
页面B

页面C
```

### 7.2.1使用props传递数据

组件的另一个用法，组件之间进行数据传递，一般的情形是父组件里嵌入子组件，子组件的形态是根据父组件传递的参数进行判别渲染和初始化，这个过程就是需要props来实现

组件中：props选项来声明需要从父级接收到数据，props的值可以是`字符串数组`另一种是对象`对象`

数组用法：

```vue
<div id='app'>
  <my-com  msg='来自父组件的数据' ></my-com>
</div>

Vue.component('my-com',{
		props:['message']
		template:`<div>{{message}}</div>`
})
```

props和data数据源的区别？

一个是来自组件，一个是来自自身，除此之外没有其他区别，都可以在method、compute、templte中使用

命名规范

* dom中使用：驼峰
* 组件命名：“-”短横连接

### 7.2.2单向数据流

通过prop传递到子组件的数据，在子组件中被改变，是会影响到父组件的，为保证不影响父这组件的值，可以已通过以下两种方式来合理的使用值

* 将props的值初始化到data中
* 通过计算属性来重新生成值

### 7.2.3props数据验证（推荐）

对需要得到的数据进行一个类型校验，错误则会提示黄色错误，带数据验证的props需要使用对象声明

关键字：

* 数据的类型配置
* 数据的默认值
* 数据是否是必传
* 自定义验证函数

```vue
props:{
	propA:Number // 必须是数字
	propB:[Number,String] // 可以是数子和字符串
	propC:{
		type:Booble,    // 布尔类型
		default:true,   //  默认值为true
		required:true   // 必传
  }
	propD:{
		type:Array|Obbjec 
		default:function(){  //默认值是对象或数组就得通过函数返回值的形式
			return Array|Objec
   },
		// 自定义验证函数
		propF:{
			validator:fucniton(value){
					return value>10
			}
		}
  }
}
```

## 7.3组件通讯



父组件 ->  子组件：props

子组件 -> 父组件：$emit

常见的组件通讯

* 父子通讯
* 兄弟通讯
* 朋友通讯

	### 7.3.1自定义事件

子传父的时候使用，这和js设计模式中观察者模式原理类似，子组件通过$emit触发，父组件通过￥on来监听子组件发出的事件，处理的方式就按照普通事件处理，区别在于监听的位置在子组件身上而已

* 监听子组件的dom事件

  需要在事件后面添加`.native`修饰符

### 7.3.2使用v-model（不常用）

Vue2.x上可以在自定义组件上使用v-model，这里头有个语法糖$emit('input',msg)  使用v-model接收

```vue
// 子组件

// 父组件
```

###  7.3.3非父子组件通讯

* 兄弟组件通讯
* 朋友组价通讯

## 7.4 使用slot分发内容

组件化开发的核心思想

* 该组件并不会知道这个部位将会是什么内容，都是由父组件自定义

通过props传递数据，event触发事件，还有slot内容分发，就构成了Vue组件的3个Api来源，不管多复杂的组件，其核心都离不开这三大核心

## 7.4.1slot用法

`默认slot`

在子组件中使用<slot>这个特殊标签，其实就是div，然后在父组件中使用slot，将自己想要的内容放进去，就像是空空的房子里放进自己的设计理念、元素搭配

例如：

```vue
<div id='app'>
  
  <child-component>
    /*父组件自定义内容*/
    <p>分发的内容</p>
    <p>更多的分发的内容</p>
  </child-component>  
 
</div>

Vue.component('child-component',{

	template:`
	
	
		
`


})
```

























