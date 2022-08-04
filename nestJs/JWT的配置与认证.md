## JWT的配置与认证

## 一、安装依赖包

```shell
$ yarn add passport passport-jwt passport-local @nestjs/passport @nestjs/jwt -S
```



## 二、添加Auth(授权)模块

```shell
$ nest g service auth logical
$ nest g module auth logical
```

1. 新建一个存储常量的文件

   在`auth`文件夹下新增一个`constants.ts`,用于储存各种用到的常量

   ```ts
   // src/logical/auth/constats.ts
   export const jwtConstants = {
     secret:'shinobi7474', // 秘钥
   }
   ```

2. 编写JWT策略

   auth文件夹下新增一个`jwt.strategy.ts`:用于编写JWT的验证策略

   ```ts
   
   
   import { ExtractJwt, Strategy } from 'passport-jwt';
   import { PassportStrategy } from '@nestjs/passport';
   import { Injectable } from '@nestjs/common';
   import { jwtConstants } from './constants';
   @Injectable()
   export class JwtStrategy extends PassportStrategy(Strategy) {
     constructor() {
       super({
         jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
         ignoreExpiration: false,
         secretOrKey: jwtConstants.secret,
   }); }
   // JWT验证 - Step 4: 被守卫调用
    async validate(payload: any) {
       console.log(`JWT验证 - Step 4: 被守卫调用`); return {
           userId: payload.sub,
           username: payload.username,
           realName: payload.realName,
           role: payload.role,
        }; 
    }   
   }
   
   ```

3. auth.service.ts：权限逻辑

   ```ts
   // /src/logical/auth.service.ts
   
   /**
    *  auto验证逻辑
    */
   
   
   import { Injectable } from '@nestjs/common';
   import { JwtService } from '@nestjs/jwt';
   import { encryptPassword, makeSalt } from 'src/utils/cryptogram';
   import { UserService } from '../user/user.service';
   
   @Injectable()
   export class AuthService {
   
       constructor(
           private readonly UserService:UserService,
           private readonly jwtService:JwtService
       ){}
   
       // Jwt验证 - step2：校验用户信息
   
       async validateUser(username: string,password: string):Promise<any>{
           console.log('jwt 验证 - strp2:校验用户信息');
   
           // 通过导入获取userInfo的方法获取到用户信息
           const user = await this.UserService.findOne(username)
   
           console.log(user,'拿到的user');
           
   
           if(user){
               // 获取到密码和密码盐
               
               const hashedPassword = user.password
               const salt = user.salt
   
               // 将密码加密后与数据库里的密码对比
               const hashPassword = encryptPassword(password,salt)
               
               if(hashedPassword === hashPassword){
                   // 密码正确
                   console.log('密码正确');
                   return {
                       code:1,
                       user,
                   }
               } else {
                   // 密码错误
                   console.log('密码错误');
                   return {
                       code:2,
                       user:null
                   }
               }
               // 查无此人
               console.log('没拿到user');
               
               return {
                   code:3,
                   user:null
               }
   
           }
           
       }
   
      // 没有证书就颁发证书
       async certificate(user:any){
           const payload = {
               username : user.username,
               sub: user.userId,
               readlName:user.readlName, 
               role:user.role,
   
           }
           console.log('jwt验证 -  step 3:处理jwt验证');
           try{
               // 根据用户信息生成token
               const token = this.jwtService.sign(payload)
               return {
                   code:200,
                   data:{
                       token
                   },
                   msg:'登录成功'
   
               }
           } catch(error){
               return {
                   code:600,
                   msg:'账号或密码错误'
               }
               
           }
       }
   
   }
   
   
   
   ```

   这里会报错：是因为还没有把 JwtService 和 UserService 关联到 auth.module.ts 中

4. 编写本地策略

   ```ts
   
   /**
    * 本地策略
    * 非必须 => 根据项目需求来判断是否需要本地策略
    */
   
    import { Strategy } from 'passport-local';
    import { PassportStrategy } from '@nestjs/passport';
    import { Injectable, UnauthorizedException } from '@nestjs/common';
    import { AuthService } from './auth.service';
    @Injectable()
    export class LocalStrategy extends PassportStrategy(Strategy) {
      constructor(private readonly authService: AuthService) {
        super();
   }
      async validate(username: string, password: string): Promise<any> {
        const user = await this.authService.validateUser(username, password);
        if (!user) {
          throw new UnauthorizedException();
        }
        return user;
      }
   }
   ```

5. 关联Module

   ```ts
   
   import { Module } from '@nestjs/common';
   import { AuthService } from './auth.service';
   import { LocalStrategy } from './local.strategy';
   import { JwtStrategy } from './jwt.strategy';
   import { UserModule } from '../user/user.module';
   import { PassportModule } from '@nestjs/passport';
   import { JwtModule, JwtService } from '@nestjs/jwt';
   import { jwtConstants } from './constants';
   
   @Module({
       imports:[
           PassportModule.register({defaultStrategy:'jwt'}),
           JwtModule.register({
               secret:jwtConstants.secret,
               signOptions:{expiresIn:'8h'} // 过期时间
           }),
           UserModule
       ],
       providers: [AuthService,LocalStrategy,JwtStrategy],
       exports:[AuthService]
   })
   export class AuthModule {}
   
   ```

   会报错就需要去 providers 数组中移除，并在 imports // src/app.module.ts

   app.module.ts ，将 AuthService 从 数组中添加 AuthModule 即可

   ```ts
   import { Module } from '@nestjs/common';
   import { AppController } from './app.controller';
   import { AppService } from './app.service';
   import { UserService } from './logical/user/user.service';
   import { UserController } from './logical/user/user.controller';
   import { UserModule } from './logical/user/user.module';
   // import { AuthService } from './logical/auth/auth.service';
   import { AuthModule } from './logical/auth/auth.module';
   
   @Module({
     imports: [UserModule, AuthModule],
     controllers: [AppController,UserController],
     providers: [AppService],
   })
   export class AppModule {}
   
   ```

6. 编写login路由

   ```ts
   import { Body, Controller, Post, UseGuards } from '@nestjs/common';
   import { AuthService } from '../auth/auth.service';
   import { UserService } from './user.service';
   import { AuthGuard } from '@nestjs/passport';
   
   @Controller('user')
   export class UserController {
   
       constructor(
           private readonly authService:AuthService,
           private readonly userService:UserService){}
   
   
   
   
       // jwt 验证 - step 1：用户请求登录
       @Post('login')
       async  login(@Body() loginParmas:any) {
           console.log(loginParmas,'参数');
            
           console.log('jwt验证 -step1: 用户登录请求');
           const authResult = await this.authService.validateUser(
               loginParmas.username,loginParmas.password
           )
               console.log(authResult,'结果代码')
           switch (authResult.code) {
   
               case 1:
                   return this.authService.certificate(authResult.user)
               case 2:
                   return {
                       code:600,
                       msg:'账号或密码不对'
                   }
               default:
                   return {
                       code:'600',
                       msg:'查无此人1'
                   }        
           }
   
       }
           
   
       // 这里有post请求
       // @Post()
       // findOne(@Body()body:any){
       //     return this.userService.findOne(body.username)
       // }
       @UseGuards(AuthGuard('jwt')) // 使用jwt进行验证
       @Post()
       async rejister(@Body()body:any){
           return await this.userService.register(body)
       }
   
   }
   
   ```

   报错：user.module.ts 将 controllers 注释掉:

   ```ts
   import { Module } from '@nestjs/common';
   import { UserService } from './user.service';
   import { UserController } from './user.controller';
   @Module({
     // controllers: [UserController],
     providers: [UserService],
     exports: [UserService],
   })
   export class UserModule {}
   ```

   此时看控制台，没有 User 相关的路由，我们需要去 去:app.module.ts将Controller 添加回

   ```ts
   import { Module } from '@nestjs/common';
   import { AppController } from './app.controller';
   import { AppService } from './app.service';
   import { UserService } from './logical/user/user.service';
   import { UserController } from './logical/user/user.controller';
   import { UserModule } from './logical/user/user.module';
   // import { AuthService } from './logical/auth/auth.service';
   import { AuthModule } from './logical/auth/auth.module';
   
   @Module({
     imports: [UserModule, AuthModule],
     controllers: [AppController,UserController],
     providers: [AppService],
   })
   export class AppModule {}
   
   ```

   这么做是因为如果在 user.module.ts 中引入 AuthService 的话，就还要将其他的策略又引 入一次，个人觉得很麻烦，就干脆直接用 app 来统一管理了。

7. 测试登录 => 会返回token给你

8. token守卫

   主要就是引入 UseGuards/AuthGuard +1

   ```ts
   import { Body, Controller, Post, UseGuards } from '@nestjs/common';
   import { AuthService } from '../auth/auth.service';
   import { UserService } from './user.service';
   import { AuthGuard } from '@nestjs/passport'; // +1
   
   @Controller('user')
   export class UserController {
   
       constructor(
           private readonly authService:AuthService,
           private readonly userService:UserService){}
   
   
   
   
       // jwt 验证 - step 1：用户请求登录
       @Post('login')
       async  login(@Body() loginParmas:any) {
           console.log(loginParmas,'参数');
            
           console.log('jwt验证 -step1: 用户登录请求');
           const authResult = await this.authService.validateUser(
               loginParmas.username,loginParmas.password
           )
               console.log(authResult,'结果代码')
           switch (authResult.code) {
   
               case 1:
                   return this.authService.certificate(authResult.user)
               case 2:
                   return {
                       code:600,
                       msg:'账号或密码不对'
                   }
               default:
                   return {
                       code:'600',
                       msg:'查无此人1'
                   }        
           }
   
       }
           
   
       // 这里有post请求
       // @Post()
       // findOne(@Body()body:any){
       //     return this.userService.findOne(body.username)
       // }
       @UseGuards(AuthGuard('jwt')) // 使用jwt进行验证 +1
       @Post()
       async rejister(@Body()body:any){
           return await this.userService.register(body)
       }
   
   }
   
   ```

   

