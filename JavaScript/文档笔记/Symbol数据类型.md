# Symbol数据类型

Symbol值：表示独一无二的值，通过Symbol函数生成，致使对象的属性名有两种类型，一种是原来就有的字符串，另一种是新增的Symbol类型，凡是属性名属于Symbol类型都是独一无二，可以保证不会与其他属性名冲突

```js
let s = Symbol()
typeof s
// "symbol"
```

注意 Symbol函数不能使用new命令 => 因为生成的Symbol是一个原始类型的值，不是对象，

由于Symbol值不是对象，所以不能添加属性，基本是它是一种类型字符串的数据类型

`Symbol`函数可以接受一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示，或者转为字符串，表较容易区分

```js
let s1 = Symbol('foo')
let s2 = Symbol('bar')
s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```



