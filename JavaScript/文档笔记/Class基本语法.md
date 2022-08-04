# Class基本语法

class语法

```js
class MyClass () {
	// class 方法
	constructor() {...}   // 构造函数
    
    // 以下时该类（系统）的方法
    method1() { ... }
    method2() { ... }
    method3() { ... }
    ... 还有很多方法
}
```

创建一个类（继承于基类）

使用  => `new MyClass()` 来创建具有上述所有方法的类

* 通过`new关键字`来创建对象  =>  `new`会`自动调用`constructor()方法，因此我们可以在constructor中初始化对象

初始化类示例

```javascript
Class User {
	constructor(name) {
		// 这个name是初始化类的时候传进来的
        this.name = name
	}
    
    sayHi() {
        alert(this.name)
    }
}

// 用法

let user = new User('林发挥')
user.sayHi() // 林发挥
```

## 什么是Class

在JavaScript中`类是一种函数`

证明：

```js
class User {
	constructor(name) { this.name = name}
	sayHI(){ alert(this.name)}
}

// Class是一个函数
alert(typeof User) // function 

// ...或者，更确切的说，是constructor方法

// 方法在prototype中，例如；
alert(User.prototype.sayHi)  // alert(this.name)

// 在原型中实际上有两个方法

alert(Object.getOwnPropertyNames(User.prototype)) // constructor ,sayHi

```

```
```

## 动态的创建类

```js
function makeClass(phrase) {
	return class {
		sayHi() {
			alert(phrase)
		}
	}
}

// 创建一个新的类
let User = makeClass('hello')

new User().sayHi() // hello
```

## Getters/setters

类可能包括getters/setters，计算属性（computed properties）等

`使用get/set实现user.name的示例`

```js
class User {
	
	constructor(name) {
		// 调用setter
		this.name = name 
	}
	
	get name() {
		return this._name
	}
	
	set name(value) {
		if(value.length < 4) {
			alert('name is too short')
		}
        this._name = value
	}

}

let user = new User('john')
alert(user.name) // john


```

## 总结 => 类语法

* 属性
* 构造器（自调用 => 用于初始化数据）
* method（方法）
* getter方法
* setter方法
* 计算

```js
class MyClass {
	prop = value  // 属性
	
	construstor( ... ) { // 构造器 }
	
	method(...) { } // method(方法)
	
	get something(...) {} //getter 方法
	set something(...) {} //setter 方法
	
	[Symbol.iterator]() {} // 有计算名称（computed name）的方法（此处为symbol）
}
```

## 严格模式

class默认使用strict严格模式执行



## 访问器

## 访问控制

* public：不受保护的属性，在类的`内部和外部都能访问到`
* protected：受保护的属性，但可在继承后在类的内部访问，有以下几种定义方式
  * 以`_`开头命名，以此告诉使用者这是一个私有变量
  * 
