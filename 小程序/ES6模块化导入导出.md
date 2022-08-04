# ES6模块化导入导出

1. export命令：用于规定模块的对外接口

   一个模块对应一个文件，文件内的对象信息外部无法获取，但可以通过export命令来暴露（所有对象信息）

   ```
   export { firstName,lastName, year}
   expost function multiply(x,y){return x * y}
   ```

2. import命令：接收一对大括号 { }=>里头指定从其他模块导入的对象信息名, { name } ===  export name

   1. 如果想为接收的变量取一个新的名字：则通过as关键字来改变 {  nameA  as  nameB }

3. export  default 命令：为模块指定默认输出

   1. 本质就是输出一个叫default的对象信息,只是系统允许他取任意名

   2. 因此它后面不能跟变量声明语句
   3. 一个模块有且仅有一个
   4. import引入时不需要大括号

   ```javascript
   export default function(){ return 'hello world'} ----- 暴露
   import random from './expost-default'        ------引入
   random() // hello world
   
   // 注释:以上import 可以用任意名称指向 expost-default输出的方法,这时就不需要原文件的名称了
   // 注意:export default ? import xx : import { xx } 
   ```

   default与as的关系

   ```javascript
   // modules.js
   function add(x,y){
   	return x * y
   }
   export { add as default } === export default add
   
   // app.js
   import { default as foo } from './modules' === import foo form './modules'
   ```

   