# 构造器和操作符new

  创建对象的方式

* let object = {  ... }     // 一般操作    （一次性数据，不系统）
* 构造函数、new 操作符             // 将相似对象抽像处理 

## 构造函数

简介：技术上是常规的函数，不过有两个约定

1. 命名：大写字母开头
2. `只能`由new操作符来执行

例如：`构造函数 + new·`可以批量生产对象

```js
function User(name) {
    this.name = name
    this.isAdmin = false
}

let user = new User('jack')
console.log(user.name,user.isAdmin)  // ject ，false
```

## new 操作符的执行

1. 一个空的对象被创建，并且分配给this

2. 函数体执行，通常会修改this,为其添加属性

3. 返回this的值

4. 用代码描述=> new User(....)

   ```js
   // 构造函数的逻辑式返回一个带初始化的d
   function User(name) {
   	// this = {} (隐式创建)   ...重点     
       this.name = name
       this.isAdmin = false
       // return this  隐式返回
   }
   ```

5. 总结，new对象时返回一个对象回来，那个对象是在构造函数里创建的一个对象，赋值上数据再返回来

