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

## 六、表单与v-model

这里主要是处理`表单类`的双向绑定，操作表单->映射到绑定的数据上

例如：单选、多选、下拉选择、输入框等等。完成数据的录入、校验、提交

最基本的就是input，可以进行双向的数据控制

```vue
<input v-model:value='value' />
data(){
	reutrn {
		value:''
     }
}
```

### 提示！

1. v-model：在输入法还没选词的时候是不会更新的，但是@input可以做到

### 6.1单选按钮



## 八、自定义指令：操作dom

vue指令:v-为前缀，v-if、v-show等，但，有些时候我们想要自定义指令来操作，dom，来满足自己的需求

## 8.1基本用法

比如注册一个v-focus的指令，用在<input>对焦

* 全局注册

  ​		`基本上需要全局的配置都在在Vue里面初始化之前配置`

  ```vue
  Vue.directive('focus',{
     // 指令选项
  })
  ```

* 页面注册

  ```vue
  var app = new Vue({
  	el:'#app',
  	directives：{
        focus:{
            // 指令选项
         }
      }
  })
  ```

  写法和组件components非常类似，以上只是实现了注册，并没有写入功能，以下是自定义指令的各个选项，`由几个钩子函数组成，且每一项都是可选的`

  * bind：只调用一次，指令第一次绑定到元素时调用，用这个钩子可以定义一个在绑定时执行一次的初始化动作
  * inserted：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）
  * update：被绑定的元素所在的模板更新时调用
  * componentUpdaed：被绑定元素所在模板完成一次更新周期时调用
  * unbind：只调用一次，指令与元素解绑时调用
  * ...
  * `每个钩子几个参数可用`
    * ​	el：指令所绑定的元素，可用用来直接操作dom
    * binding：一个重要的信息对象
      * name：指令名，不包含前缀
      * value：指令的的绑定值，例如：v-my-dir:1+2,则value为3
      * oldValue：指令绑定的前一个值，仅在update、componentUpdated钩子可以用
      * expression：绑定值得字符串新形式：v-my-dir:1+1则为1+1
      * arg：传给指令的参数，v-my-dir:foo,arg的值就是foo
      * modifiers：一个包含修饰符的对象，例如：v-my-dir.foo.bar,修饰符对象modifiers的值为{foo:true,bar:true}
      * vnode：Vue编译生成的虚拟节点，进阶篇介绍
      * oldVnode：上一个虚拟节点，仅在update和componentUpdated钩子中可以用

  ### 总结

  绝大数场景下，我们都会在bind钩子中绑定一些事件，例如document上使用addEventListener绑定然后在unbind中使用removeEventListener解绑，如果需要传入多个值，自定义指令也 支持传入一个JavaScript对象字面量，只要合法类型就行，例如

  ```vue
  
      <div id='app'>
          <div v-test="msg:'hello'"></div> 
      </div>
  
  
  	 Vue.directive('test',{
  
          bind:function(el,binding,vnode){
  
  			console.log(binding.value.msg)
  
     		 })
  
  
       var app = new Vue({
  
          el:'#app',
          data:{
              message:'some text'
          }
  
       })
  ```

  

  

  



