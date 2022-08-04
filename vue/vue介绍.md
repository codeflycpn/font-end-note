# vue介绍

## 1、声明式渲染

语法模板 => 将输入渲染到Dom

```vue
<div id="app">
  {{message}}
</div>
```

```'
var app = new Vue({
	 el:'#app',
	data:{
		message:'hello vue'	
	}
}
```

## 2、条件与循环

基本用法：v-if、v-show

高级用法：三目运算、三目嵌套运算(使用计算属性)

```vue
<div v-if='index === 1'>
  为真才能看到我
</div>
```

循环 v-for

高级：遍历二维数组、根据type属性来渲染不同的结构

```vue
<div class='item' v-for='(item,index) in list' :key='index'>
  {{item}}
</div>
```

## 3、处理用户输入

* v-on：触发事件：有很多事件，具体看文档
* v-model：绑定在input中，使得input输入的内容，一一的传输到v-model绑定的变量上

## 4、组件化应用构建（预制模板）



