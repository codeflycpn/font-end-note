# Map和Set

## Set

Set：类似数组，但是`成员唯一`没有重复的元素

`Set`本身是一个构造函数,用来生成Set数据结构

```js
const s = new Set()
[2,3,4,5,6,7,7,5].forEach(x=>{
	s.add(x)
})

for(let i of s) {
	console.log(i)
}
// 2,3,4,5,7
```

1. 通过add()添加成员，结果表明Set结构不会添加重复的值
2. Set函数可以接受一个数组(iterable接口的其他数据结构)用来初始

```js
// 例1
const set = new Set([1,2,3,4,5])
[...set]
// [1,2,3,4]

// 例2
const item = new Set([1,2,3,4,5,5,5,5])
item.size = 5

// 例3
const set = new Set(document.querySelectorAll('div'))
set.size// 56

// 类似于
const set = new Set()
document
	.querySelectorAll('div')
	.forEach(div => set.add(div))
set.size // 56
```

例1和例2都是Set函数接受数组作为参数，例3是接受类似数组的对象作为参数

展示数组去重的方法

```js
[...new Set(array)]
```

上面的方法也可以取出字符串例的重复字符

```js
[...new Set('ababbc')].join('')
// 'abc'
```

向Set加入值的时候，不会发生类型转换，所以s和's'是两个不同的值，Set内部判断两个值是否相等，使用的算法叫做"Same-value-zero equality" 它类似精确运算符`===`，主要区别是想Set加入值时`NaN`等于自身，而精确相等运算符认为`NaN`不等于自身

代码演示

```js
let set = new Set()
let a = NaN
let b = NaN
set.add(a)
set.add(b)
set // Set{NaN}
```

添加两次NaN但是只有一个NaN说明在Set内部，两个NaN是相等的

另外两个对象总是不相等的

```js
let set = new Set()
set.add({})
set.size // 1

set.add({})
set.size // 2
```

Set实例的属性和方法

* 属性
  * Set.prototype.constructor：构造函数，默认就是Set函数
  * Set.prototype.size：返回Set实例的成员总数

* 方法    =>    两大类（数据操作和遍历方法）：
  * 数据操作
    * add：添加某个值，返回Set结构本身
    * delete：删除某个值，返回一个布尔值，表示是否删除成功
    * has：返回一个布尔值，判断该元素是否为Set成员
    * clear：清除所有成员，没有返回值
  * 遍历方法
    * keys：返回键名
    * values：返回键值
    * enries：返回键值对
    * forEach：回调遍历所有成员
    * for ...  of ：支持该种遍历方式

注意：

* Set遍历顺序就是插入顺序，很关键，比如使用Set保存一个回调函数的列表，调用时就能保证按照添加顺序调用

* Set结构没有键名，只有键值，或者说键名和键值是同一个值

遍历的应用

扩展运算符（...）内部使用`for..of`循环,所以也可用于Set结构

```js
let set = new Set(['red','green','blue'])
let arr = [...set]
//['red','green','blue']
```

扩展运算符和Set结构相结合，就可以去除数组重复成员

```js
let arr = [3,4,2,2,5,5]
let uni = [...new Set(arr)]
// [3,4,2]
```

数组的map和filter和Set结合

`map方法`不会改变原来数组的值

```js

let set = new Set([1,2,3])
set = new Set([...set].map(x => x*2))
// 返回Set结构：{2,4,6}
let set = new Set([1,2,3,4,5])
set = new Set([...set].filter(x =>(x % 2) == 0 ))
```

Set实现并集、交集、差集  => Union、Intersect、Difference

```js
let a = new Set([1,2,3])
let b = new Set([4,3,2])

// 并集
let union = new Set([...a,...b])   // 将a和转数组在放置Set完成去重
// Set {1,2,3,4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)))
// set {2,3}

// a相对于b的差集
let difference = new Set([...a].filter(x => !b.has(x)))
// set{1}
```

如果想在遍历操作中，同步改变原来的Set结构，目前没有直接的方法，但是有两种间接的方法

* 利用原Set结构映射出一个新的结构，然后赋值给原来的Set结构
* 利用`Array.form`方法

```js
// 方法一
let set = new Set([1,2,3])
set = new Set([...set].map(val => val *2))
// set的值是2,4,6

// 方法二
let set = new Set([1,2,3])
set = new Set(Array.from(set,val=>val * 2))   // form 方法转成数组
// set的值是2,4,6
```



## WeakSet

后续补充

## Map

简介：js中的对象本质上是键值对的集合(Hash结构)`但是对象的键只能是字符串`，

形成："字符串=>值" 这种的形式，Map:`值=>值`，是一种顶层Hash结构，那么Map可以取代对象了吗？

Map的基本使用：

1. 基本的数据处理

```js
const m = new Map()
const o = {p:'Hello world'}

m.set(o,'content'),
m.get(o) // content
m.has(o) // true
m.delete(o) //true
m.has(o) // false

```

2. 通过构造函数初始化

   ```js
   const map = new Map([
   	["name",'张三'],
   	["title",'Author']
   ])
   // 初始化之后直接使用
   map.size // 2
   map.has("name") // true   这是怎么取出来的
   map.get('name') // 张三
   map.has("title") // true
   map.('title') // Author
   ```

   该map在初始化的时候就指定了两个键（'name','title'）

   该map构造函数接受数组作为参数，实际上执行了下面的算法

   ```js
   const items = [
   	["name",'张三'],
   	["title",'Author']
   ]
   
   const = new Map()
   
   // 将数组的数据写入进去
   items.forEach([key,value])=>{
       map.set(key,value)
   })
   ```

   问题1:forEach[ ]形式的

   结论：不仅仅是数组，任何具有iterator接口，并且每个成员都是一个双元素的数组的数据结构，都可以当做Map构造函数的参数，这就是说，`Set`和`Map`都可以用来生成新的Map

   ```js
   const set = new Set([
   	['for'，1],
   	['bar',2]
   ])
   const m1 = new Map(set)
   m1.get('foo') //1
   const m2 = new Map([['baz',3]])
   const m3 = new Map(m2)
   m3.get('baz')
   ```

3. 多次赋值，后者覆盖前者

4. 获取一个没有的键，则返回`undefind`

   ```js
   new Map().get('asdfgh')  // undefind		
   ```

   两个值一个样的实例，由于`指针地址的不同`所获取到键值也是不一样的

   ```js
   const map = new Map（）
   const k1 = ['a']
   const k2 = ['a']
   // 这里的键是一样的
   map
   .set(k1,111)
   .set(k2,222) 
   .set(k1,111) // 111
   .get(k2,222) // 222
   // 虽然这里的键名是一样的但是两个不同的地址，所指向的对象也就不一样了
   ```

   总结：Map的键实际上是跟内存地址绑定的，只要键的内存地址，而并非表面的值，作为键与键之间的区别：`解决问题`同名属性碰撞（clash）的问题，当我们扩展到别人的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名了

   

5. 键名严格相等

   Map的键是简单类型的值（数字、字符串、布尔值）则只要这个值严格相等，Map将视为一个键,比如0和-0，布尔的true和字符串的true就是不同的键



6、Map的实例属性和操作方法

（1）size属性

`size`属性返回Map结构的成员总数

```js
cosnt map = new Map()
map.set('foo'，true)
map.set('bar',false)
map.size // 2
```

(2)Map.prototype.set(key,value)`set方法`

`set`方法设置键名`key`对应的键值为`value`,然后返回值整个Map结构，如果`key`已经有值了则键值就会被更新，否则就生成该键

```js
const m = new Map()
m.set('eda',5)			// 键是字符串
m.set(222,'wodefa')		// 键是数值
m.set(undefind，'hhh')  // 键是undefind
```

(3)Map.prototype.get(key,value)`get方法`

`get`方法获取key对应的键值，如果找不到`key`,返回`udnefind`

```js
const m = new Map()
const hello = function(){
	console.log("hello")
}
m.set(hello,'hello ES6') // 键是函数
m.get(hello) // Hello Es6
```

（4）Map.prototype.has(key)

`has`方法返回一个布尔值，表示该键是否在当前Map中

```js
const m = new Map()
m.set(1,'是是是')
const array  = []
m.set(array,222)

m.has(array) // true
m.has(2) // false
```

（5）Map.prototype.delete(key)

`delete`方法是删除某个键,返回`true`如果删除失败,则返回`false`

```js
const m = new Map()
m.set(undefind,'nth')
m.has(undefind) // true
m.delete(undefind)
m.has(undefind) // false
```

（6）Map.prototype.clear(key)

`clear` 方法清楚所有的成员，没有返回值

```js
let map = new Map()
map.set('foo',true)
map.set('bar',false)

map.size // 2
map.clear()
map.size // 0
```

7、遍历方法（`三个遍历器和一个遍历方法`）

* `Map.prototype.keys()`: 返回键名的  =>   遍历器
* `Map.prototype.value()`：返回键值的 => 遍历器
* `Map.prototype.entries()`：返回所有成员的 => 遍历器
* `Map.prototype.forEach()`: 遍历Map的所有成员

`注意`

Map的遍历就是插入顺序

```js
cosnt map = new Map([
	['F','no'],
	['t','yes']
])
// 这好像是Map的原始结构

for (let key of map.keys()){
	console.log(key) // 'F' 't'
}

for (let value of map.values()) {
	console.log(value) // 'no' 'yes'
}

for (let item of map.entries()) {
	console.log(item[0],item[1]) // "f" "no" "t" "yes"
}
// 第二种
for (let (key,value) of map.entries()) {
	console.log(key,value) // "f" "no" "t" "yes"
}
// 等同于
for (let  [key,value] of map) {
	console.log(key,vlaue) //  "f" "no" "t" "yes"
}
```

最后的那个例子，表示Map结构的默认遍历器接口（Symbol.iterator）属性就是`entries`方法

```js
map[Symbol.iterator] === map.entries // 后续思考
```

8、Map结构转为数组结构，比较快速的方法是使用扩展运算符（...）

```js
const map = new Map([
	[1,'one'],
	[2,'two'],
	[3,'three']
])
[...map.keys()]
// [1,2,3]
[...map.values()]
// 'one' 'two' 'three'
[...map.entries()]
// [[1,'one'],[2,'two'],[3,'three']]
[...map]
// [[1,'one'],[2,'two'],[3,'three']]
```

还有一种,结合数组的`map`方法、`filter`方法可以实现Map的遍历和过滤（Map本身没有map和filter方法）

```js
const map = new Map()
	.set(1,'a')
	.set(2,'b')
	.set(3,'c')
const map1 = new Map([
	[...map].filter(([k,v]) => k < 3)
])
// 产生 Map结构 {1 => 'a', 2=> 'b'}

const map2 = new Map([
	[...map].map(([k,v]) => [k*2,'_' + v])
])
// 产生Map结构 {2=>'_a',4=>'_b'}
```

`Map`foreach方法

```js
map.forEach((value,key,map) => {
	console.log("key:$s,Value:$s",key,value)
})
```

`forEach`方法还可以接受第二个参数，用来绑定`this`

```js
const reporter = {
	report: function(key,value) {
		console.log("key:%s",Value:%s,key,value)
	}
}
map.forEach(function(value,key,map){
	this.report(key,value)
},reporter)
```

forEach方法回调函数`this`，就指向`reporter`

8、Map与其他数据结构的相互转换

1. Map转数组  => ...扩展运算符

```js
const myMap = new Map()
	.set(true,7)
	.set({foo:3},['abc'])
[...myMap]
// [[true,7],[{foo:3},['abc']]]
```

1-1 数组转Map

```js
new Map([
	[true,7],
	[{foo:3},['bac']]
])
// 得到的Map
// Map{
//	true=>7
//	object {foo:3} => []'abc']
//}
```

2. Map转对象

   `如果的Map的键都是字符串` => 无损转换成字符串

   ```js
   function strMapToObj(strMap) { 
   	let obj = Object.create(null)
   	for(let [k,v] of strMap) {
   		obj[k] = v
   	}
   	return obj
   }
   const map = new Map()
   	.set('yes',true)
   	.set('no','false')
   strMapToObj(map) // {yes:true:no:false}
   ```

   如果有非字符串的键名，那么这个键名就会被转成字符串，再作为对象的键名

2-2 对象转Map

object.entries()

   ```js
   let obj = {"a":1,"b":2}
   let map = new Map(object.entries(obj))
   ```

手动实现

```js
function objToStrMap(obj){
	let StrMap = new Map()
	for (let k of Object.keys(obj)) {
		strMap.set(k,obj[k])
	}
	return strMap
}
objToStrMap({yes:true},no:false)
// Map{"yes" => true, "no" => false}
```

3、Map转为JSON

Map转JSON要区分两种情况，一种情况是，Map的键名字符串，这时可以选择转为对象JSON

```js
function strMapToJson(strMap) {
	return JSON.stringify(strMapToObj(strMap))
}
let myMap = new Map().set('yes',true).set('no',false)
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

3-3 JSON转Map

JSON转为Map正常情况下，所有的键名都是字符串

```js
function jsonToStrMap(jsonStr) {
	return objToStrMap(JSON.parse(jsonStr))
}
jsonToStrMap('{"yus":true,"no":false}')
```

另一种情况：整个JSON就是一个数组,且每个数组的成员本事，又是一个有两个成员的数组，这时，它可以一一对应的转为Map，这往往是Map转为数组JSON的逆操作

```js
function jsonToMap(jsonStr) {
	return new Map(JSON.parse(jsonStr))
}
jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map { true => 7, object {foo:3} => ['abc']}
```
