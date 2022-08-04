# TypeScript

> 基础数据类型
>
> * boolean
> * number
> * void
> * string
> * undefind、null

总结:



## 一、基础部分

js数据类型分为两类：原始数据类型、对象类型

* 原始数据类型：基本六大类 + symbol + Bigint

### 1、原始数据类型的使用（TS）

1. boolean（布尔值）

   ```ts
   let isDone:boolean = false  
   
   //编译通过，如果没报错，那就是通过 
   ```

   注意：使用构造函数创建的对象不是布尔类型

   ```ts
   let createByBoolean:boolean = new Boolean(1)
   
   // 报错
   // 在ts中，boolean是基本类型 而Boolean属于构造函数,其他类型一样的道理
   ```

2. 数值

   使用`number`定义数据类型

   ​		Ts语法

   ```ts
   let declteral:number = 6
   let hex:number = 0xf00d 
   // ES6中二进制表示法
   let binaryLiteral:number = 0o744 
   let notANumber:number = NaN  
   let infinityNumber:number = Infinity  
   ```

   ​		转成Js语法

   ```js
   var declteral = 6
   var hex = 0xf00d;
   var binaryLiteral = 10; // 将二进制转10精制
   var notANumber = NaN;
   var infinityNumber = Infinit
   ```

   ​        其中 0b1010 和 0o744 是 ES6 中的二进制和八进制表示法，它们会被    编译为十进制数字

3. 字符串

   使用`string`定义字符串类型

   ```Ts
   let myName: String = 'TOm'
   let myAge:numver = 25
   // 模板字符串 => 与ES6一样
   let sentence:string = `Hello,my name is ${myName}
   I,11 be ${myAge + 1} years old next month`
   
   ```

4. 空值

   JavaScript没有空值(void)的概念,Ts中`void`表示没有任何返回值的函数

   ```ts
   function alertName(){
       alert('my name is Tom')
   } 
   ```

   声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`（只在 --strictNullChecks 未指定时

5. Null和Undefined

   在TS中，可以使用`null`和`undefined`定义这两个原始类型

   ```ts
   let u:undefind = undefind
   let n:null = null
   ```

    

## 2、任意值

任意值（Any）表示允许被赋值为任意类型

如果你是非Any类型，定义后，就不允许赋值其他类型，但是任意类型就可以

1. 任意值的属性和方法

   在任意值上访问任何属性都是允许的

   ```ts
   let anyThing:any = 'hello'
   console.log(anyThing.myName)
   console.log(anyThing.myName.firstName)
   ```

   也允许调用任何方法

   ```ts
   let anyThing: any = 'Tom';
   anyThing.setName('Jerry');
   anyThing.setName('Jerry').sayHello();
   anyThing.myName.setFirstName('Cat');
   ```

   **声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值**

2. 未申明类型的变量 => 任意类型

   ```ts
   let something;
   something = 'seven';
   something = 7;
   something.setName('Tom');
   // 等价于
   let something: any;
   something = 'seven';
   something = 7;
   something.setName('Tom');
   ```

## 3、类型推论

如果没有指定类型，那么，TS将依据类型推论的规矩来推出一个类型

```ts
let myFavoriteNumber = 'seven'; // 这不是Any类型 这算是类型推论
myFavoriteNumber = 7;
// 等价于
let myFavoriteNumber:string = 'seven'
myFavoriteNumber = 7  // 所以这种操作是错误的
```

## 4、联合类型

取值可以是多种类型:通过`|`分割

```ts
let myFovoriteNumber:string | number
myFovoriteNumber = '王德发'
myFovoriteNumber = 25
myFovoriteNumber = false // 报错
// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.

```

1. 访问联合属性的方法

   我不确定你是那种类型，而每种数据类型提供的方法又不一样

   ```ts
   function getLength(something:string|number):number{
       return something.length
   }
   // 报错
   // index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
   //   Property 'length' does not exist on type 'number'.
   ```

   something是String的方法不是number的方法所以会报错，只能访问这两种数据类型的公共方法toString才行，这里内核也没做优化

2. 联合类型在赋值的时候会被类型推论推出一个类型，再改类型基础之上再进行操作

   ```ts
   let bar:string|number
   bar = '王德发' // 类型推断成String类型
   bar = 7 // 类型推断成Number类型
   ```

## 5、对象类型 --- 接口

在ts中使用接口（Interfaces）定义对象类型

**接口**:它是对行为的抽象，而具体是需要类去实现

例如:

```ts
// 接口名称首字母大写
interface Person {
    name:string;
    age:number;
}
let son:person {
    name:'王德发',
     age:25
}
```

​		我们都做了啥，首先定义了一个son接口，再定义了一个son类型并且初始化属性，**这就是约束了son的形状与son接口一致**

1. 不按照接口规范定义

   ```Ts
   interface Preson {
       name:String;
       age:number;
   }
   // 错误示范
   let son1:Preson {
       name:'大儿子',
       age:15,
       height:178  // 接口没有这个属性
   }
   // 错误示范
   let son2:Preson {
       name:'二儿子'
       // 缺少接口属性
   }
   ```

   赋值的时候，口径统一，属性都得有，不能多也不能少

2. 可选属性（个别属性不要求统一）就定义成可选属性

   ```ts
   interface Person {
       name: string;
       age?: number; // 通过在属性的后面就+?
   }
   // 正确 ，age属性是可选属性
   let tom: Person = {
       name: 'Tom'
   };
   ```

3. 任意属性

   我时候我希望该接口有一个任意属性

   ```ts
   interface Person {
       name:string;
       age?:number;
       [propName:string]:any; // 定义该任意属性类型为Stirng：值为any
   }
   let tom:Person {
       name:'tom',
        gender:'male'
   }
   
   //任意属性的类型为any||联合属性：他要求其他属性得是他的字属性
   ```

   

   `一个接口中只能定义一个任意属性`。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

   ```ts
   ```

   

   

4. 只读属性

   有时候我们希望，某个属性被初始化后就不能更改的值

   ```ts
   interface Person {
       readonly id:number; // 这就是readonly类型的属性
       name:string;
       age?:number;
       [propName:string]:string;
    
   }
   let tom:Person{
       id:8096,
       name:'TOm',
       gender:'male'
   }
   tom.id = 9527 // 错误，这已经是只读类型，不允许修改
   // index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
   ```

## 6、数组的类型

定义数组的方式有很多种

基本方式:数据类型+[ ]

```ts
let fibonacci:number[] = [1,2,3,4,5,6]

let fibonacci:number[] = [1,2,3,4,5,6,'王德发'] // 错误
fibonacci.push('8') // 错误
```

1. 数组泛型

   通过使用数组泛型（ArrayGeneric）` Array<elemType>`表示数组

   ```ts
   let fibonacci:Array<number> = [1,1,2,3,4]
   ```

2. 接口数组

   ```tsx
   interface NumberArray{
   	[index:number]:number
   }
   ```

3. 类数组

   不是数组类型，比如`arguments`

   ```ts
   function sum(){
       let args:number[] = arguments
   } 
   
   // arguments只是一个类接口，不能用其描述
   
   function sum() {
       let args: {
           [index: number]: number;
           length: number;
           callee: Function;
       } = arguments;
   }
   
   
   // Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
   ```

4. any在数组中的应用

   常用的

   ```ts
   let list:any[] = [1,2,'王德发',false,null,{name:'x'}]
   ```

   

## 6、函数类型

1. 函数声明

   * 函数声明

     ```ts
     function sum(x,y){return x + y}
     ```

   * 表达式声明

     ```ts
     const sum = function(x,y){return x + y}
     ```

2. TS对函数的约束

   * 输入和输出都要约束

     ```ts
     function sum(x:number,y:number):numver{
         return x + y
     }
     ```

   * 传入的参数个数定死

     ```ts
     sum(1,2,3) // 多了一个参数
     sum(1) // 少了一个参数
     ```

3. 函数表达式

   * 正常的写法

     ```ts
     let sum = function(x:number,y:number):number{
         return x + y
     }
     ```

   * 约束的写法

     ```ts
     let sum:(x:number,y:number) => number =function(x:number,y:number):number{
         return x + y
     }
     ```

   *  ts中的`=>`用来表示函数的定义

     * 左边：输入类型
     * 右边：输出类型

4. 接口定义函数的形状

   ```ts
   interface SearchFunc{
       (source:string,subString:string):boolean
   }
   
   let MySearch:SearchFunc
   mySearch = function(source:string,subString:string){
       return source.search(subString)!== -1
   }
   ```

5. 可选参数

   ```ts
   function buildName(firstName:string,lastName?:string){
       return lastName?lastName??firstName
   }
   ```

   注意：**可选参数后面不允许再出现必需参数了**，它得是最后一个

6. 参数默认值

   ```ts
   function(firstName:string='cat',lastName:string){}
   ```

7. 剩余参数：`...rest`

   ...rest,只能放在最后

   ```ts
   // ES6
   function push(array,...item){
       item.forEach(function(item){
           array.push(item)
       })
   }
   let a:any[] = []
   
   //TS
   function push(array:any[],...items:any[]) {
       item.forEach(item=>{
           array.push(item)
       })
   }
   
   push(a,2,3,4)
   ```

8. 重载

## 7、类型断言

手动指定一个值得类型

语法：

`值 as 类型`  || `<类型>值`，推荐使用第一种

1. 类型断言的用途

## 8、声明文件

## 9、内置对象

内置对象：根据全局作用域上存在的对象，这里的标准是ECMAScript + 其他环境（Dom、BOM）的标准

* ECMAScript的内置对象

  `Boolean`、`Error`、`Data`、`RegExp`、...更多内置对象

  在ts中定义这些对象

  ```ts
  let a:Booleran = new Booblean(1)
  let b:Error = new Error('Error occurred')
  let c:Date = new date()
  let d:RegEXP = /[a-z]/
  ```

* DOM和BOM的内置对象

  `Document`、`HTMLElement`、`Event`、`NodeList`、

  ts中常用的类型

  ```ts
  let body:HTMLElement = document.body
  let aliDiv:NodeList = document.querySelectorAll('div'){// Do something}
  ```

* Ts核心库的定义文件

  其中定义了，所有浏览器环境需要用到的类型，并且已经预制到ts内核中

  ```ts
  Math.pow(10,'2')
  
  // index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
  ```

  `Math.pow`必须是接受两个number类型的参数，事实上`Math.pow`的类型定义如下

  ```ts
  interface Math {
         /**
       * Returns the value of a base expression taken to a specified power.
       * @param x The base value of the expression.
       * @param y The exponent value of the expression.
       */
      pow(x:number,y:number):number
      
  }
  ```

  DOM中的例如

  ```ts
  document.addEventListener('click',function(e){
      console.log(e.targetCurrent)
  })
  // index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.
  ```

  ```ts
  interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
      addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
  }
  ```

  所以 `e` 被推断成了 `MouseEvent`，而 `MouseEvent` 是没有`targetCurrent` 属性的，所以报错了。

  注意，TypeScript 核心库的定义中不包含 Node.js 部分

## 10、用TypeScript写Node.js

Node.js不是内置对象的一部分，如果想要在Ts中写Node.js,则需要第三方声明文件

```js
npm install @types/node --save-dev
```











