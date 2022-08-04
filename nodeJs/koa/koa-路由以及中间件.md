# koa2

## 一、koa2开始

1. 环境搭建

   ```
   node.js:v7.6以上
   nvm管理多版本node.js：可以用nvm 进行node版本进行管理
   windows系统安装nvm https://github.com/coreybutler/nvm-windows
   Ubuntu系统安装nvm https://github.com/creationix/nvm
   npm 版本3.x以上
   ```

2. 初始化pockage.json

   ```
   npm init 
   ```

3. 安装koa2

   ```
   npm install koa
   
   hello worle 代码
   // 引入koa包
   const koa = require('koa')
   // 创建koa实例
   const app = new Koa() 
   // 使用中间件
   app.use(async (ctx) =>{
   	ctx.body = 'hello koa2'
   })
   app.listen(3000)
   console.log('[dome] start-quick is starting at post 3000')
   ```

   4. 启动Koa2的dome

      由于koa2是基于async/await操作中间件,目前node.js7.x的harmony模式下才能使用,所以启动脚本如下

      ```
      node index.js
      ```

##  二、async、await的使用

1. async：让该方法变成异步

2. await等待异步方法执行完成,会造成阻塞就像加载JavaScript脚本一样

3. 详细说明

   **async是让方法变成异步**，在终端里用node执行这段代码，你会发现输出了Promise { ‘Hello async’ }，这时候会发现它返回的是Promise(这是一个异步处理器)。

   ```javascript
   async function testAsync(){
   return 'Hello async';    // 这是一个普通函数
   }
   const result = testAsync();
   console.log(result);
   
   
   PS E:\code\BXShop> node async.js
   Promise { 'Hello async' }  // 这是一个异步处理器
   ```

   **await** **在等待async方法执行完毕**，其实await等待的只是一个表达式，这个表达式在官方文档里说的是Promise对象，但是它也可以接受普通值。 注意：await必须在async方法中才可以使用因为await访问本身就会造成程序停止堵塞，**所以必须在异步方法中才可以使用**。

   ```javascript
   function getData(){
   return 'zhangsan';
   }
   async function testAsync(){
   return 'Hello async';
   }
   async function test(){
   const v1=await getData();
   const v2=await testAsync();
   console.log(v1,v2);
   }
   test();
   ```

   **async+await同时使用**

   async 会将其后的函数（函数表达式或 Lambda）的返回值封装成一个 Promise 对象

    await 会等待这个 Promise 完成，并将其 resolve 的结果返回出来。

   async+await的作用是：你是一个异步任务，我会等你执行完成并且返回数据后我在操作

   被await后不会执行，形成异步任务同步化

## 三、koa目录结构分析

 	1. lib
      	1. application.js  //是整个koa2的入口文件,封装了context,request,response,以及最核心的中间处理
      	2. context.js     // 处理应用上下文,里面封装部分request.js和response的方法
      	3. request.js      // 处理http请求
      	4. response.js   // 处理http响应
 	2. package.json
 	3. 以上目录了解即可,通常我们使用脚手架生成的目录