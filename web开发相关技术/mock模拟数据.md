# mock模拟数据

1. 安装

   ```js
   # 安装
   npm install mockjs
   ```

2. 使用

   首先要根据需求生成数据，再用ajax请求

   1. 生成数据

      ​		注意格式，要配置一个api接口路径，然后定义数据

      ```js
      import Mock from 'mockjs'
      // 随机生成的对象
      Mock.mock('http://localhost:4000/test',{
          "userInfo|4":[{    // 生成4个如下格式名字的数据
              "id|+1":1,  // id从当前数开始后续依次加一
              "name":"@cname",    // 名字为随机中文名字
              "ago|18-28":25,    // 年龄为18-28之间的随机数字
              "sex|1":["男","女"],    // 性别是数组中的一个，随机的
          }]
      })
      ```

   2. 请求数据

      通过ajax请求数据

      ```js
      export function mock1(){
      	return 			requestFunc('http://localhost:4000/test')
      }
      ```


## 常用模拟数据格式

1. 个数

   `"user|2"`：2个

   `'user|1-5'`：2~5个

2. 模拟数据库

   比如模拟名字有个专用名字库：@cname

   模拟一段文字/英文：@paragraph

   
