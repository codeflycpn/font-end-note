## F.prototype

`JavaScript一开始就有原型继承`=> 这是JavaScript核心特性之一

F.prototype：指的是F的一个`名为=>prototype的属性`

```js
let animal = {
	eats:true
}
// 够着函数 配合new关键字构造出对象
function Rabbit(name) {
	this.name = name
}
Rabbit.prototype = animal // 执行继承操作

let reabbit = new Rabbit('white Rabbit')

alert(rabbit.eats) // true
```

代码解释：

1. `Rabbit.prototype = animal`

   * 当执行一个Rabbit对象时候（new Rabbit）把他的[[Prototype]]赋值给animal

   * 图解

     ```js
     Rabbit		-> prototype    animal{eats:true}
     
     									↑ [[prototype]]
     							 rabbit{name:"white Rabbit"}        
     ```

     * prototype是一个水平走势，表示一个常规属性
     * [[Prototype]]是垂直的，表示`rabbit`继承自`animal`

## 默认的F.ptototype,构造器属性

每个函数都有"`prototype`"属性，即使我们没有提供给它

默认的`prototype`是一个只有`constructor`的对象，属性`constructor`指的是函数本身

如下

```js
funciton Rabbit() {}
/* default prototype
	Rabbit.prototype = {constructor:Rabbit}
*/
```

检测

```js
function Rabbit(){
	// by default
	// Rabbit.prototype = {constructor:Rabbit}
}
alert(Rabbit.prototype.constructor == Rabbit) // true
```

使用`constructor`属性创建对象（这里保存了构造函数本身，初始化操作）

```js
function Rabbit(name) {
	this.name = name
	alert(name)
}
let rabbit = new Rabbit('white Rabbit')
let rabbit2 = new rabbit.constructor('Black Rabbit')
```

`用途`：当我们有一个对象不知道他是由哪个构造器，并且我们需要创建类似的对象时，用这种方法就很对方方便

`constructor`特性：仅存在于函数默认的prototype属性中，之后怎么处理都行，也可能被替代

例如

```js
function Rabbit(){
	Rabbit.prototype = {
		jumps:true
	}
}
let rabbit = new Rabbit()
alert(rabbit.consturctor === Rabbit) // false
```

`最好的做法是`增/减属性到默认的prototype中，而不是将其整个覆盖

 ```
 functino Rabbit(){
 	// 不要将整个prototype覆盖
 	// 可以向其添加内容
 	Rabbit.prototype.jumps = true
 	// 默认的Rabbit.prototype.constructor被保留下来的 => constuctor保留了原始构造器
 }
 ```

`手动创建Constructor`属性

```
Rabbit.prototype = {
	jumps:true,
	constructor:Rabbit
}
```

## 总结

通过构造函数创建对象设置`[[Prototype]]`的方法

特性：

* `F.prototype`属性（非`[[Prototype]]`）在`new F`被调用为新对象的`[[Prototype]]`复制

* `F.prototype`的值要么是一个对象，要么就是`null`；其他的值都不其作用

* `prototype`：属性仅在设置一个构造函数（constructor function）并通过`new`调用时，才具有这种特殊的影响

  在常规对象上，`prototype`没有什么特别的

  ```js
  let user = {
  	name:"john",
  	prototype:"Bla-bla" // 这里的普通的属性
  }
  ```

  默认情况下：所有的函数

  `F.prototype = {contructor:F},`所以我们可以同故宫访问它的"`constructor`"属性来获取来一个对象的构造器

习题解析

```js
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};
let rabbit = new Rabbit();
alert( rabbit.eats ); // true
```

```js
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

Rabbit.prototype = {};

let rabbit1 = new Rabbit();

alert( rabbit.eats); // ?  ture
alert( rabbit1.eats); // ? undefind
```



解析：`Rabbit.prototype`的赋值操作=>为新对象设置了[[prototype]],但它不影响已有的对象

```js
function Rabbit() {}
Rabbit.prototype = {
	eats.true
}
let rabbit = new Rabbit()

Rabbit.prototype.eats = false

alert(rabbit.eats) // false

```

解析：`同上解析`



```
function Rabbit() {}
Rabbit.prototype = {
	eats:true
}
let reabbit = enw Rabbit()

// delete关键字用来删除属性，并且返回删除情况

delete rabbit.eats

alert(rabbit.eats)? // true 

```

解析：`你只是删除了自己的属性而已` 但是你并没有这个属性，但也无妨

 ```js
 function Rabbit() {}
 Rabbit.prototype = {
   eats: true
 };
 
 let rabbit = new Rabbit();
 
 delete Rabbit.prototype.eats;
 
 alert( rabbit.eats ); // ? undefined
 ```

解析：属性 `eats` 被从 prototype 中删除，prototype 中就没有这个属性了。



## 成为另外又给它

```js
funciton User(name) {
	this.name = name
}
let user = new User('hello')

// 创建另外一个它

let user2 = new user.constuctor('hello1')

// 抹掉原生基因
user2.prototype = {}

// 改变基因

let user3 = new user2.constuctor('报错了')
```

