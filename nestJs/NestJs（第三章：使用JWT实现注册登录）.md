# NestJs（第三章：使用JWT实现注册登录）

jwt介绍：全称，json web token，是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准，特别适用于分布式站点的单点登录（sso）场景，JWT的声明用于身份提供者和服务提供者间被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其他业务逻辑所必须的声明信息，该Token也可被用于认证或加密



`具体原理`：《JSON Web Token - 阮一峰》



## Jwt实现登录流程

1. 客户端进行登录请求
2. 服务端拿到请求，根据参数查表
3. 若没有匹配到用户，将用户信息进行签证，并颁发Token
4. 客户端拿到Token后，储存到某一地方，在之后的请求中都得带上token
5. 服务端，接收到带Token的请求后，直接根据签证进行校验，无需查询用户信息

### 编写加密工具函数

用于加密解密

```ts
import * as crypto from 'crypto';
      /**
       * Make salt
       */
      export function makeSalt(): string {
        return crypto.randomBytes(3).toString('base64');
}
/**
* Encrypt password
* @param password 密码 * @param salt 密码盐 */
      export function encryptPassword(password: string, salt: string): string {
        if (!password || !salt) {
return ''; }
        const tempSalt = Buffer.from(salt, 'base64');
        return (
// 10000 代表迭代次数 16代表长度
          crypto.pbkdf2Sync(password, tempSalt, 10000, 16, 'sha1').toString('base64')
        );
1. }
```



## 用户注册

1. 