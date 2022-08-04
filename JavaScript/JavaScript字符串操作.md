# JavaScript字符串常用操作

> 目录
>
> 1. 字符串截取
>    * substring(index>0,endindex/null)：下标截取
>    * slice(index,endindex/null)：下标截取 -1为最后一个
>    * substr(index,number)：下标+个数截取
>    * split()：指定字符分割成数组
> 2. 字符串查找类
>    * indexOf() => includes() && search（多个正则匹配）：
>    * 根据指定字符查找，index或返回-1，后面那个返回bool，
>    * lastIndexOf()：根据指定字符查找，等于indexOf的倒序走位
>    * match()方法：返回一个数组
> 3. 字符串其他归类
>    * replace()：字符串替换
>    * 模版字符串法：字符串和变量拼接
>    * 组合其法：多中处理数据的方法组合使用
>    * string转成数组法

## 一、字符串截取

1. substring()：指定下标

   * 接收两个参数（start，end/null）备注：不能为负值
   * 返回一个新字符串

2. slice() =>substring

   * 接收两个参数（start，end/null）备注：-1就是最后一个以此类推

3. substr()：指定个数

   * 接收两个参数（start，end/nul(个数)

4. split() ** 

   * 切割字符串，返回数组

   ```javascript
   let str = 'www.gitee.com'
   str.split((以xx来分割),偏移量)
   // 参数1:可以是正则表达式
   // 参数2:1(放回数组的第一个)...null(返回全部数组)
   // 配合join方法可以实现多个散件重组
   ```

## 二、字符串查找

1. indexOf()&includes()(*Es6)
   * indexOf((需要查找的串),(从哪里开始))：检索指定字符/串首次出现的位置
     * 返回下标
     * 返回-1
       * includes()处理更加优雅...备注:区分大小写
2. lastIndexOf():`与indeOf相反`返回最后的位置，下标也是也是倒序的
3. serch()：
   * 检索字符串中指定的子字符串，或检索与正则相匹配的子字符串
   * 返回首次子字符串出现的位置
4. match()：在字符串内检索指定的值，或找到一个或多个正则表达式的匹配
   * 没找到，返回null
   * 找到，返回一个数组
     * 【0】存放匹配文本
     * [...]还有两个对象属性,index和input
   * 全局匹配(匹配多个串,而不仅第一次出现的的地方)
     * 数组中只有数据,没有index和input对象属性

## 三、字符串其他方法

1. replace()：字符串替换操作：参数（（子串 / 正则） , (替换成什么)）
   * 没有进行正则全局替换则`只会替换第一个`
   * 返回替换过的字符串
   * toLowerCase() 大写替换成小写
   * toUpperCase()将小写替换成大写
2. 模板字符串(es6)
   1. 可以将变量放进字符串`${}`
   2. 可以换行
3. 组合其法：利用过个转化语法来实现的一套最终解决方案
4. String转为数组
   1. 转成以为数组
      * 通过sqlit就可以了
   2. 转成二维数组