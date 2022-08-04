# 模块（Module）

一块内容涉及到大量代码块，就可以使用模块文件来进行业务分割，全部写在工程文件中非常臃肿

ES6之前的主流模块化

* AMD		   --- 最古老的模块系统，`require.js`库实现
* CommonJS --- node.js服务器创建的模块系统
* UMD           --- `建议作为通用模块系统`，同时兼容AMD和CommonJS

## 什么是模块

一个模块就是一个js文件

一个脚本就是一个模块

`特性:`模块之间可以`相互引用`=>通过`export导入出`和`import引入`

`例如:`

```js
// 模块导出

export function sayHi(user) {
	console.log('Hello' + user)
}
```

```js
// 导入 + 使用

import { sayHi } from './sayHi.js'   // import指令 通过相对路径加载模块 + 分配空间（变量）给导入的函数

alert(sayHi)	// funciotn ...
sayHi('jonny')	// hello jonny
```



## ES6=> import + export

 导出方式：三种 export

* 逐一导出

  ```js
  export cosnt name = '王德发'
  export function getUser(){}
  ```

* 多个统一导出（有人说优先使用这种，效果是逐一导出一致）

  ```js
  cosnt time = '五月'
  function getInfo () {}
  export {
  	time,getInfo
  }
  ```

* 默认导出

  ```js
  const address = '巴黎市区'
  
  // 只能导出一个数据
  export default address
  ```

模块加载：import

​		import接受一对大括号{}，指定要其他模块导出的变量名称，必须与对外导出的module.js导出的变零名称一致，想要自定义接受变量名称通过`import { name:newName } from './a.js'`

1. 拿到的接口数据只读的，不可以拿到之后进行逆向改变

   ```js
   import {a} form './a.js'
   // 修改a的值
   a = {}  // Syntax Error : 'a' is read-only;
   ```

2. 修改其他模块的值（不推荐使用）不好排错：前提是a对象而不是接口

   ```js
   import {a} from './a.js'
   a.foo = 'hello'
   ```

   

* 原生html加载文件示例

  ```js
  <script src='aaa.js' type='module'/>
  ```

* import导入三种方式

  1. 逐一导出  +  多个统一导出

     ```js
     import { name,getUser} from './a.js'  // 括号是必须要有
     ```

   2. 默认导入

      ```js
      import self from './b.js'  // 通过default导出的可以自命名
      ```
  
   3. ​	全部导出
  
      ```js
      import *as aaa from '/a.js'
      
      console.log(aaa.name, aaa.address)
      ```
  
  ## ES6模块 as关键字
  
  export输出的变量是本来的名字，但是可以使用`as`关键字`重命名`
  
  ```js
  const name = '王德发'
  funtion v1(){...}
  
  export {
  	name as newName,
      v1 	 as newv1
  	v1   as newnewv1
   }
  // v1 可以搞分身
  ```
  
  主意，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应的关系
  
  ```js
  export 1 // 报错
  
  var m = 1 
  export m  // 报错
  ```
  
  
  
  ## ES6模块from
  
  form是模块的路径：相对路径、绝对路径、配置路径
  
  配置路径：必须要告诉js引擎，加载路径，例如node_modules
  
  ```js
  import { myMethod } from 'util'
  ```
  
  * import 具有提升效果，不管写在哪里都会第一执行
  
  * import 是静态执行，不使使用变量操作（表达式、if、变量）
  
    ```js
    import {'f'+'oo'} from 'my_module'
    
    let modele = 'my_module'
    import {foo} from modele
    
    //条件导入
    if(true){
        import {foo} from './a.js'
    }else {
        import {bar} from './b.js'
    }
    ```
  
  * import  可以不套出任何接口
  
    ```js
    import 'lodash'
    ```
  
  * 同一模块只会加载一次
  
    ```js
    import 'a'
    import 'a'
    ```
  
  * 最后
  
    通过Babel转码，CommonJs模块的require命令和ES6命令可以写在同一模块里，但是不要这样做
  
    ，因为，import是最快加载的，有时候不会得到预期结果
  
  ## ES6整体加载
  
  除了指定输出某个接口，还可以进行整体加载通过`*`指定一个对象，所有输出的值都会加载在这个对象上面
  
  ```js
  // 一定是导出的
  export const name = '王德发'
  export function getName () {}
  
  // 导入
  import { name,getName } from './a.js'
  // 全部导入
  import * as allInterface form './a.js'
  console.log(allInterface.name) // 王德发
  ```
  
  ## ES6模块 export default 
  
  
  
  在没有了解全该文件所有的接口又想要快速使用的情况下可以使用 ,
  
  `他就是输出一个叫default的变量` => 这个变量导入时可以自己命名，`显然一个模块只能有一个默认输出`
  
  1. export default ：为模块`指定默认输出`
  
     ```js
     // a.js
     export default function(){
         console.log('foo')
     }
     ```
  
     导入使用
  
     任意名称指向`a.js`输出的方法
  
     ```js
     imoprt 不需要括号+自定义名称 from './a.js'
     
     new() // 就是默认导出的函数
     ```
  
     注意：
  
     ```js
     export default 42 // default的本质是将后面的值赋值给default所以是正确的
     
     export 50  // 这是错误的写法，这不是一个接口类型，导入时无法接收到，接口规范可以了解一下
     
     ```
  
  2. 接受默认+正常导出的接口
  
     ```js
     import _,{name,getInfo} from './a.js'
     ```
  
  ## ES6模块高级篇
  
  > export + import 的复合写法
  >
  > 模块的继承
  >
  > 跨模块常量
  >
  > import()

1. export + import （转发）：统一导入源，将所有源导入到a.js后统一导入到某个模块中

   ```js
   export {foo,bar} form './c.js'
   
   import {foo,bar} from './c.js'
   export {foo,bar}
   ```

   
