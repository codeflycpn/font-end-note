## Spread运算符 =>数组+对象

一、在log中使用

```js
let a = [1,2,3,4]
cosnole.log(...a) // 1234 而不是 [1,2,3,4]
```

二、深度复制

* 复制数组

```js
let a = [1,2,3,4]
let b = [...a].push(5) // [1,2,3,4,5]
```

* 复制对象

```js
let user = {name:'john',age:20}
let userCopy = {...user} 
```

三、数据合并

* 数组合并

```js
let a = [1,2,3,4]
let b = [5,6,7,8]
let c = [...a,...b] // 1,2,3,4,5,6,7,8
```

* 对象合并  =>重复的元素后者覆盖前者

```js
let user1 = {name:'john',age:20}
let user2 = {name:'ran',salary:'20k'}
let user3 = {...user1,...user2} 
```

四、作为参数传递

```js
function sum(a,b) {
	return a + b
}
let num = [1,2]
sum(...num) // 3
```

与Math函数一起使用

```js
let num = [1,2,3,4,5]
Math.min(...num)
Math.max(...num)
```

五、结构变量中使用

```js
let [melon,...fruits] = ['xy','mg','li','xhs']
melon;
fruits;
```

```js
let user = {name:'ram',age:20,salary:'20k',job:'tester'}
let { name,age,...datails } = user
name; // ram
age;20
datails; // {salary:'20k',job:'tester'}
```

六、NodeList对象转为数组

```js
let nodeList = document.querySelectorAll('.class')
let nodeArray = [...nodeList]
```

七、字符串转为数组

```js
let name = 'ram'
let a = [...name] // ['r','a','m']
```

八、数组去重

```js
let num = [1,2,3,3,3,4,4,5]
let a = [ ... new Set(num)]  //1,2,3,4,5  
// 1、将num转Set去重
// 2、通过...将类数组的结构转数组
```

