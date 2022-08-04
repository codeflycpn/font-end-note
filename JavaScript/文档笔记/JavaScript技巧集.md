# JavaScript技巧集

## 一、非数字转换为数字

​	js是松散类型的语言，我们不必显示指定变量类型

​	js还可以根据使用上下文自由的转换值类型

​	实现方式1：`一元 + 运算符`

```js
+"42"   // 42
+ true  // 1
+ false // 0
+ null  // 0
```

​	实现方式2：`Number(value)`

​	作用：将其他值转成Number类型，如果不行则返回NaN

```js
Number('42')   // 42
Number('1.2')  // 1,3	
Number('tax')  // NaN
```

​	实现方式3：`parseInt()`

​	将String作为第一个参数和转换String的基础，并且将始终返回整数

```js
parseInt('1234',10)       // 1234
parseInt('11 playes',10)  // 11
parseInt('player 2',10)   // NaN   必须以数字开头
parseInt('10.81')         // 10    => 使用parseFloat()可以拿到小数
```

## 二、对象管理

`解构`会经常用到、他允许我们从对象中提取数据 + 将数据分配给变量

```js
const  user = {age:10,sex:'男'}
const  {age} = user  // 10
// 需要重命名变量
const {newAge:age} = user

```

 通过函数返回的对象进行解构，然后选择要使用的值

```js
function getParson(){
	return {
		firstName = 'Max',
		lastName = 'Best',
		age:42
	}
}
const { age } = getParson()
console.log(age) // 42
```

`解构`可以让函数返回多个值

删除对象属性并将剩余属性传递到新的变量中

```]
const {age:_,...parson} = getParson()
console.log(parson) // {firstName = 'Max',lastName = 'Best',}
```

## 交换两个变量

```js
let me = 'happy',you = 'sad'
[me,you] = [you,me]  // 将其结构成相反的变量
//me = 'sad', you = 'happy'
```

## 设置默认值

变量默认参数：??：空合并运算符，当左侧为null或undefind时返回右侧内容

```js
const bookList = receiveBook ?? []  // 有点像三目   
```

函数默认参数：

```js
function add (width,height = 100) {
	return widht + height
}
console.log(add(50))  //150
```

解构对象设置默认值

```js
const rectangle = {height:400}
const {height = 750 , widht = 500 } = rectangle
console.log(height) // 400
console.log(widht) // 500
```

`只有在新对象的某个值为undefined的情况下结构才会有效`

## 区间随机数

Math.random()

```js
let num = parseInt(Math.random()*(max-min+1)+min,10)  // 加min是防止小于min
```

## 数组去重

1. Set对象：
2. spread(...运算符)

```js
const nuiqueArray = [...new Set(array)]  // ...将Set转成数组，Set将数组去重
```

## 动态属性名称（应用在快速动态创键）

计算的属性名称，允许`对象`字面量的`属性键`使用表达式

可以在方括号 [  ] 中存放key，我们可以将变量用作属性键

```js
const type = 'fruit'
const itme = {
	[type]: 'kiwi'
}
console.log(item) // {fruit:'kiwi'}
```

[ ] 访问对象

```js
item[type]: // kiwi
item['fruit'] // kiwi
```

## 获取函数参数的长度

```js
arguments.length
```

## 获取数组后面的元素：slice

```js
let a = [1,2,3,4,5]
a.slice(-1) // 5
a.slice(-2) // 4,5
....
```

## 缩短数组

1. slice

2. array.length = 3

   ```js
   let a = [1,2,3,4,5]
   a.length = 2
   console.log(a) // [1,2]
   ```

## 对象 + Spread（...运算）

1. 实现将两个对象合并

   ```js
   let obj1 = {'age1':14,'sex':'男','des':'福州'}
   let obj2 = {'iq':150,'sg':'189cm'}
   const addObj = {...obj1,...obj2} // 这就完成了合并
   ```

   

## 查找数组元素的索引

```js
let a = [1,2,3,4,5,6]
a.indexOf(3) // 2
```

## 展平二维数组

```js
let a = [1,2,[3,4],5,6]
let b = a.flat()
b // [1,2,3,4,5,6]
```

## 填充数组

```js
const RandomIntArray = (min, max, n = 1) => Array.from({ length: n }, () => Math.floor(Math.random() * (max - min + 1)) + min)

RandomIntArray(5, 20, 4) // [ 8, 17, 13, 9]
```

## 获取当前对象的类型

```
```

## 获取当前网站的地址

```js
let url = window.location.href
```

## 计算某元素在数组中出现的次数

```js
const CountOcc = (array, val) => array.reduce((x, v) => (v === val ? x + 1 : x), 0);

console.log(CountOcc([3, 3, 4, 1, 2, 5, 3, 6],3)) // 3

console.log(CountOcc([3, 4, 4, 1, 3, 6],4)) // 2 
```

## 错误处理

## 修剪空格

```js
let s = 'b  bcds d d'
let t = s.replice(/\s+/g)
```

## 数字化整数

```js
const DigitizeInt = n => […${n}].map(i => parseInt(i));

DigitizeInt(4560) // [4, 5, 6, 0]

DigitizeInt(131) // [1,3,1] 
```

