# JavaScript日期常用操作

> 目录
>
> 1. 创建日期对象
>
>    1. 当前日期
>    2. 指定日期
>    3. 日期与时间戳相互转换
>
> 2. 获取日期的方法
>
> 3. 将日期转成时间戳的三种方式
>
> 4. 日期格式化
>
>    

## 一、创建日期对象

首先要生成对象，然后才有后续

* 方式1：new date()：获取当前日期
* 方式2：new date(毫秒数|| 时间格式)：
  * 获取指定日期 ,尽量使用///因为兼容性更好
  * 通过parse()和UTC()方法区别在于参数的格式不一样
  * 作用：进行时间的一个对比，所以这个对象需要创建多个传入不同时间参数进行对比

## 二、获取日期的方法

1. 年月日
   * getDate：天（1~31）
   * getDay：返回星期（0~6）
   * getMounth返回月份（0~11）
   * getFullYear返回年份（YYYY）
2. 时分秒
   * getHours：返回小时（0~23）
   * getMinutes：返回分钟（0~59）
   * getSeconds：返回对象的秒数（0~59）
   * getTime()返回1970年1月1日至今的毫秒数（多用于日期比较）**
3.  按需求返回
   1. UTC:1997到指定日期的毫秒数
   2. toSting:将date日期转成字符串=>用于显示
4. set系列设置日期

## 三、将日期转成时间戳的三种方式

​	参数都是日期格式:例如:2/2/1997,返回毫秒数

1. date.getTime()：精确到毫秒

2. date.valueOf()：精确到毫秒,返回日期的毫秒数。因此可以方便使用`比较`操作符（大于或小于）来比较日期

3. Date.parse(date)：精确到秒

4. date.utc()::1997到指定日期的毫秒数

5. now()方法:此时此刻的毫秒数

   使用介质不同会有兼容性问题

   1. 比如:let box = new Date('wodefake') 返回Invalid Date 谷歌返回Nan firefox返回Nan
   2. ios就不兼容gettime(1996-6-7)兼容///

## 四、日期格式化

1. toLocaleString() ：根据本地时间格式，把 Date 对象转换为字符串。 例如 2017/3/3 上午11:23:45

2. date.getTime()：1488511425056

   ```javascript
   var box = new Date();
   alert(box.getFullYear() + '-' +(box.getMonth()+1) + '-' + box.getDate() +
    ' ' +box.getHours() + ':' + box.getMinutes()+':'+box.getSeconds()); 
   // 2017-5-19 19:38:33
   ```

   