# css常用伪类

x:child系列：x的父元素的子元素

x:nth:child系列：x的父元素的第x个子元素（与child基本一样）（第几个,不论元素的类型）

x:of-type系列：x的父元素的首个x元素（第一个<p>，只认元素类型）

1. 父元素的第一个/最后一个|子元素:first-child、:first-child

   ```css
   /* 例如 */
   1.p:first-child p的父元素的首个子元素
   1.P:last-child  p的父元素的最后一个子元素
   2.p:first-child i p的父元素的首个子元素里的i
   3.**常用<p>中的第一个<i>：p > i:first-child { color:red }  翻译：p里面的第一个i
   ```

2. 我是我父元素的第一个元素:first-of-type、:last-of-type

   ```css
   p:first-0f-type:选择的这个p元素是其父元素的第一个p元素
   p:last-of-type:选择的这个p元素是其父元素的第一个p元素
   ```

5. 选择某元素的父元素的第n个子元素:nth-child(n)、:nth-last-child(n)

   ```css
   p:nth-child(n)选择所有p元素的父元素的第二个子元素，n可以是个数字、关键词、公式
   ```

6. 选择某元素的父元素的倒数第n个元素

   ```css
   p:nth-last-child(2) 选择p元素倒数的第二个子元素
   ```

8. 选择所有某元素的倒数第n个为某的子元素

   ```css
   p:nth-last-of-type(2)选择所有p的第二个为p的子元素
   ```

   

