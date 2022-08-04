## 对象方法 => this

通常创建对象用来表示真实世界的实体，例如用户、订单等

```js
let user = {
	name: '林发挥'，
	age: 30
}
```

方法实例

```js
let user = {
	name: '林发胡',
	age: 30
}
// user 对象的属性是个函数 => 我们称之为方法
user.sayHi = function() {
	console.log('Hello')
}() // 立即执行 输出 Hello
```

* 我们使用函数表达式创建了一个函数 => 将其指定给对象的user.sayHi属性
* 随后我们使用user.sayHi()调用它，就可以让它说话了
* `作为对象属性的函数`我们称之为方法
* 我们得到了user对象的sayHi方法

`在几乎所有的情况下较短的语句是首选`

## 方法中的this（我所在的这个对象（上下文）的属性/方法）

`通常` => `方法`是需要`访问`对象中`属性`才能完成工作

例如：user.sayHi()中的代码就可能需要用到user的name属性

`访问对象的属性的方式=>`this：this的值就是点该方法的那个对象 => user.sayHi() 即this就是user

```js
let user = {
	name: 'John',
	age: 30,
	
	sayHi() {
	// this 指的是当前的对象
	alert(this.name)	// this 替代  user，
  
	}
}
user.sayHi() // John
```

总结：`this`在对象的方法中调用对象的变量原本应该是`对象.(属性)`可用`this关键字代替`

## 箭头函数没有自己的this

箭头函数的`this`的值`取决于上一层函数的值`

```js
let user = {
	firstName = '林发挥'
	sayHi() {
		let arrow = () => { alert(this.firstName) }
        arrow()
	}
}

user.sayHi() // 林发挥
```

## 总结

* 存储在对象中的函数   => `方法`
* 方法可以`将对象引用为this`

`this`的值实在程序运行时得到的

* 一个函数在声明时，它可能已经调用了`this`但这时候的`this是没有值的`，只有在`函数运行时`才会有值