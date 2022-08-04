# qq登录原理



原理：对接第三方系统，比如qq登录，支付宝支付登录，一般使用oauth2授权方式

1,[OAuth 2.0 的一个简单解释](https://link.segmentfault.com/?enc=P6HqRSt2q4W6aibzT7Zqaw%3D%3D.Qnk%2FAZopTRQFsAUcI%2BQYAMLjG8yV%2BgMTzogGk9%2Fl78IFKrR9XwmcKjCzZiGhWAWXwza8vSNxkw2Ec0NkCdengA%3D%3D)
2,[理解OAuth 2.0](https://link.segmentfault.com/?enc=ZzdL6EHN%2Bw5sFq4fwG%2Fn6w%3D%3D.pgmkaLF7UWmqCVDlRXqi%2FgsBcLh%2FkxNkdTUZX%2BBOeRxx2T6TQWBFQ70UQxRQFjFexVGH3nJqBs7YgX6CH7hQpQ%3D%3D)
3, [OAuth 2.0 的四种方式](https://link.segmentfault.com/?enc=4%2BIm%2BJDVx7oOEwtJ4F1adw%3D%3D.SRf0RucGb8PWjssRFpG8Esn%2BpvRUC7pNmFSeY9XW%2B0H418GBV4ZLZ5PMgzlS%2Fp0VRyqkYawk%2BaSLp2W3I2Fg3g%3D%3D)

## 基本知识

window.location.href



## 为什么？以及怎么做

qq用户的信息：

* 头像、昵称
* qq号：全网唯一标识

## 为什么需要oauth2

你通过qq登录之后会给你`appid`||`client_id`后续就是靠这个作为认证

* 正常登录流程思路

  1. 调用getUserInfo接口

  2. 再经过用户同意的情况下，才会把用户信息返回给你

     * 如何让用户同意？（跳网页）=> oauth2规范

       通过location.href定位到qq授权页面：[https://graph.qq.com/oauth2.0](https://link.segmentfault.com/?enc=IXHsBS7hIsfBdLmXCpoEfw%3D%3D.nmUEAa9yhPRdRlEkv%2FlTpnoJBpB3hjoMoUaBD4s3EFo%3D)

## 最终的授权流程

`用户再该页面点击头像确认后，只需要做两件事`

1. 传入登录之后要去的页面
2. 传getUserInfo请求参数给我

* 

## 人类思维优化版

首先你在url里面放置queryString是不太安全的，这里提供了两种思路

1. 数据分离法（前端）：url只返回一个code，然后你再根据这个code请求数据

   数据分离法（后端）：根据这个code，调用腾讯接口，取到json格式的请求参数（accessToken），之后

   再根据accessToken去获取json格式的userinfo信息

2. 自己想