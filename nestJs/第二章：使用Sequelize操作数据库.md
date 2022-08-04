# 第二章：使用Sequelize操作数据库

1. 安装mysql
2. 安装Navicat Premium数据库管理工具

## 一、项目的数据库配置

1. 创建文件夹`config`：与src同级，`用于系统级配置`

2. 然后在config文件夹下创建`db.ts`

   ```ts
   // config/db.ts
   const productConfig = {
       mysql: {
       port: '数据库端口',
       host: '数据库地址',
       user: '用户名',
       password: '密码',
       database: 'nest_zero_to_one', // 库名 
       connectionLimit: 10, // 连接限制
       }, 
   }
   
   const localConfig = {
       mysql: {
       port: '数据库端口',
       host: '数据库地址',
       user: '用户名',
       password: '密码',
       database: 'nest_zero_to_one', // 库名
       connectionLimit: 10, // 连接限制
       }, 
   };
   // 本地运行是没有 process.env.NODE_ENV 的，借此来区分[开发环境]和[生产环境]
    const config = process.env.NODE_ENV ? productConfig : localConfig;
   
   
   // 导出一个开发环境的数据库对象
   export default config;
   ```

   

3. 安装Sequelize依赖包，市面上有很多数据库连接工具

   ```shell
   npm i sequelize sequelize-typescript mysql2 -S
   
   yarn add sequelize sequelize-typescript mysql2 -S
   
   ```

4. 创建数据库管理文件：src/database，然后再database下创建sequence.ts

   ```ts
   // src/database/sequelize.ts
         import { Sequelize } from 'sequelize-typescript';
         import db from '../../config/db';
         const sequelize = new Sequelize(
           db.mysql.database,
           db.mysql.user,
           db.mysql.password || null,
   {
          // 自定义主机; 默认值: localhost host: db.mysql.host, // 数据库地址 // 自定义端口; 默认值: 3306
   port: db.mysql.port,
   dialect: 'mysql',
   pool: {
   max: db.mysql.connectionLimit, // 连接池中最大连接数量
   min: 0, // 连接池中最小连接数量
   acquire: 30000,
   idle: 10000, // 如果一个线程 10 秒钟内没有被使用过的话，那么就释放线程
   },
   timezone: '+08:00', // 东八时区 },
   );
   // 测试数据库链接 sequelize
           .authenticate()
           .then(() => {
   console.log('数据库连接成功'); })
   .catch((err: any) => {
   // 数据库连接失败时打印输出 console.error(err); throw err;
   });
   export default sequelize;
   ```

    

## 二、数据库连接测试

