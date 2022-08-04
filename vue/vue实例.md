# vue实例

简介：每一个vue应用都是通过vue函数够造出来的，`传入一个选项对象`

```vue
let vm  = new Vue({

	// 选项

})
```

## 2、模板语法

Mustache语法：渲染内容

```vue
<span>Message:{{msg}}</span>
```

Attribute处理：动态给标签/组件传值

```vue
<div v-bind:id='dynamicId'></div>
```

JavaScript表达式

```vue
{{ number + 1}}
{{ ok?'yes':'no' }}
{{message.split('').reverse().join('')}}
<div v-bind:id="'list - 'id"></div>
```

## 3、指令

v-前缀的特殊attribute，指令attribute的值预期是单个JavaScript表达式

指令：当表达式的值发生改变时，将其连带的影响 => 响应式的作用于dom，例如v-if

* 参数（一般指令都会接受一个参数，作为爆发前的衡量）

```vue
  <a v-bind:href='url'>..</a>
  // href是参数
```

```vue
  <a v-on:click='doSomeThing'>
```

* 动态参数

  ```vue
  <a v-on[click/focus/...]='doSomeThing'></a>
  ```

* 修饰符

  是以半角句号`.`指明的特殊后缀，用于指出一个指令应该以特殊方式绑定
  
  例如：.prevent修饰符告诉v-on指令触发事件调用event. preventDefault()

  ```vue
  <form v-on:submit.prevent='onSumbit'>...</form>
  ```

## 4、语法糖

v-bind

```vue
<!-- 完整语法 -->
<a v-bind:href='url'>...</a>
<!-- 语法糖 -->
<a :href='url'>..</a>
<!--动态参数-->
<a :[key]='url'>..</a>
```

v-on

```vue
<!-- 完整语法 -->
<a v-on:click='event'>...</a>
<!-- 语法糖 -->
<a @click='event'>..</a>
<!--动态参数-->
<a @[key]='event'>..</a>

```



