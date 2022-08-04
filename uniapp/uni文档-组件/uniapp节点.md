# uniapp节点



```js
uni.createSelectorQuery() // 返回一个SelectorQuery 对象实例
```

SelectorQuery 对象实例方法包括

* select等，选择节点的方法
* boundingClientRect等，选择需要查询的信息



## 定位选择器的选取范围

```js
selectorQuery.in(component) // 一般in(this)  

// 意思是将选取的范围定位到组件components内，返回一个SelectorQuery实例

// 初始时，选择器仅选取页面范围的节点，不会选取任何自定义组件中的节点

```

实例

```js
const query = uni.createSelectorQuery().in(this);
query.select('#id').boundingClientRect(data => {
  console.log("得到布局位置信息" + JSON.stringify(data));
  console.log("节点离页面顶部的距离为" + data.top);
}).exec();
```

解释：

selectorQuery.select(selector)：选择的是第一个匹配到的选择器，返回一个nodeRef实例对象

selector：

- ID选择器：`#the-id`
- class选择器（可以连续指定多个）：`.a-class.another-class`
- 子元素选择器：`.the-parent > .the-child`
- 后代选择器：`.the-ancestor .the-descendant`
- 跨自定义组件的后代选择器：`.the-ancestor >>> .the-descendant`
- 多选择器的并集：`#a-node, .some-other-nodes`

selectorQuery.selectAll(selector)

在当前页面下选择匹配选择器 `selector` 的所有节点，返回 `NodesRef` 数组对象，

selectQuery.selectorViewport()：定位到该区域是什么节点

选择显示区域，可用于获取显示区域的尺寸、滚动位置等信息，返回一个 `NodesRef` 对象实例。

selectQuery.exec(callback)

执行所有的请求。请求结果按请求次序构成数组，在callback的第一个参数中返回。
