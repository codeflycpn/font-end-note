# JavaScript数组常用操作

> 目录
>
> 1. concat：数组连接
>
> 2. join：没看懂
>
> 3. push：在末尾添加一个或多个元素，返回长度
>
> 4. unshift：向数组头部添加一个或多个元素，返回长度
>
> 5. shift：删除第一个元素并返回
>
> 6. pop：删除最后一个元素并返回
>
> 7. slice：截取数组元素（start，end）不包含这两个index，返回新的数组，不改变原始数组
>
> 8. splice：删除从index~n个元素，返回被修改后的数组
>
> 9. sort：数组排序，没那么智能
>
> 10. reverse：翻转数组，会改变原始数组
>
> 11. every：给数组的每一个元素进行体侧，只有全部过关才会返回true
>
>     ```javascript
>     function isBig(ele,index,array){
>     	return ele<10
>     }
>     [1,2,3,4,5].every(isBig); //true
>     ```
>
> 12. some： 和every一样，区别在于通过一个就返回true
>
> 13. filer：数组的每一个元素进行选拔，通过的元素组成精英元素并且返回
>
> 14. map：数组的每一个元素进行培养，最终返回新数组，注释带有return的都是浅拷贝，返回假指针
>
> 15. forEach：操作某个数组中的元素
>
> 16. ES6新增:
>
>     1. find：查找某个值，传入一个回调函数（找到第一个后返回值并停止查找,与filter类似）
>
>     2. 
>
>     3. findIndex：查找某个值，传入一个回调函数（找到第一个后返回下标并停止查找）findIndex：
>
>     4. fill：元素替换，其实就是删除+添加操作
>
>     5. form：将类似数组的对象转成数组
>
>     6. of：将一组值转成数组，同时弥补了Array构造函数的缺陷
>     
>        ```javascript
>        Array.of(1,2,3) // [1,2,3]
>        Array.of(2) // [2]
>        array(3)//[,,,] 
>        ```
>     
>     7. inlcudes：判断该数组中是否有某元素返回bool并indexOf的-1要优雅一点

