## Echart概念篇

## 1.图标容器及大小

1.1 `初始化`的时候定于一个html容器定义好宽高，这就是图标默认的尺寸，但是也可以通过`opts.width/height`来重写尺寸

```js
  var myChart = echarts.init(document.getElementById('main'), null, {
     // 初始化容器
    width: 600,
    height: 400
  });
```



1.2 `响应式`容器大小

我们希望图表的宽度始终是保持100%，通过window.onresize监听浏览器尺寸的变化

```js
  window.onresize = function() {
    myChart.resize();
  };

```

```js
myChart.resize({
  width: 800,
  height: 400
});
```

注意api接口文档的定义方式

1.3 容器节点被销毁和重建

一个页面有多个图表也就是多个节点，在控制显示隐藏的时候，移除dom之后，再次显示，但是你的图表不会再出现了，已经被移除掉了

做法：当要销毁节点的时候调用`echartsInstance.dispose`方法，这样做法是并没有完全销毁，然后再调用`echart.init`初始化

## 2. 样式

* 颜色主题
* 调色盘
* 直接样式设置（itemStyle、lineStyle、areaStyle、label、....）
* 视觉映射（visualMap）

## 2.1 修改全局样式方式

Echart5除了默认主题外，还内置了`dark`主题

```js
var chart = echarts.init(dom, 'dark')
```

其他的主题链接https://echarts.apache.org/theme-builder.html



自己配置主题后再导入

```js
// 假设主题名称是 "vintage"
$.getJSON('xxx/xxx/vintage.json', function(themeJSON) {
  echarts.registerTheme('vintage', JSON.parse(themeJSON));
  var chart = echarts.init(dom, 'vintage');
});
```

如果是umd格式的js文件

```js
// HTML 引入 vintage.js 文件后（假设主题名称是 "vintage"）
var chart = echarts.init(dom, 'vintage');
// ...
```

## 2.2 调色盘

可以在option中设置它给定了一组颜色，图形、系列会自动从其中选择颜色，可以是`全局的调色盘`，也可以设置自己的`专属调色盘`

```js

option = {
  // 全局调色盘。
  color: [
    '#c23531',
    '#2f4554',
    '#61a0a8',
  ],
  series: [
    {
      type: 'bar',
      // 此系列自己的调色盘。
      color: [
        '#dd6b66',
        '#759aa0',
        '#e69d87',
        '#8dc1a9',
        '#ea7e53',
      ]
      // ...
    },
    {
      type: 'pie',
      // 此系列自己的调色盘。
      color: [
        '#37A2DA',
        '#32C5E9',
        '#67E0E3',
        '#96BFFF'
      ]
      // ...
    }
  ]
};
```



## 2.3直接样式设置：（itemStyle、lineStyle、areaStyle、label、....）

直接的样式设置是最常用的设置方式，纵观option，有很多地方设置itemStyle、lineStyle、areaStyle、lable，这些东西可以直接设置图形元素的颜色、线宽、点的大小、标签文字，标签样式等

## 高亮的样式：emphasis

 

## 3.数据集

数据一般在series属性中

```
option = {
  xAxis: {
    type: 'category',
    data: ['Matcha Latte', 'Milk Tea', 'Cheese Cocoa', 'Walnut Brownie']
  },
  yAxis: {},
  series: [
    {
      type: 'bar',
      name: '2015',
      data: [89.3, 92.1, 94.4, 85.4]
    },
    {
      type: 'bar',
      name: '2016',
      data: [95.8, 89.4, 91.2, 76.9]
    },
    {
      type: 'bar',
      name: '2017',
      data: [97.7, 83.1, 92.5, 78.1]
    }
  ]
};
```

这种数据模型的优点是：

### 3.1数据集的第二种方式

将数据集配置在dataset属性中

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    // 提供一份数据。
    source: [
      ['product', '2015', '2016', '2017'],
      ['Matcha Latte', 43.3, 85.8, 93.7],
      ['Milk Tea', 83.1, 73.4, 55.1],
      ['Cheese Cocoa', 86.4, 65.2, 82.5],
      ['Walnut Brownie', 72.4, 53.9, 39.1]
    ]
  },
  // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
  xAxis: { type: 'category' },
  // 声明一个 Y 轴，数值轴。
  yAxis: {},
  // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]
};
```

第二种格式

```js
option = {
  legend: {},
  tooltip: {},
  dataset: {
    // 用 dimensions 指定了维度的顺序。直角坐标系中，如果 X 轴 type 为 category，
    // 默认把第一个维度映射到 X 轴上，后面维度映射到 Y 轴上。
    // 如果不指定 dimensions，也可以通过指定 series.encode
    // 完成映射，参见后文。
    dimensions: ['product', '2015', '2016', '2017'],
    source: [
      { product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7 },
      { product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1 },
      { product: 'Cheese Cocoa', '2015': 86.4, '2016': 65.2, '2017': 82.5 },
      { product: 'Walnut Brownie', '2015': 72.4, '2016': 53.9, '2017': 39.1 }
    ]
  },
  xAxis: { type: 'category' },
  yAxis: {},
  series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }]
};
```



## 4.数据转换 -> transform进行数据转换

将一个数据集通过转换生成一份新的数据集，映射到图表中

## 5. 坐标轴

直角坐标系中的x、y轴都由以下四部分组成

1. 轴线
2. 刻度
3. 刻度标签
4. 轴标题四部分组成

x轴一般用于表示维度、数据的类别，y轴则是数据值的体现

```js
option = {
  xAxis: {
    type: 'time',
    name: '销售时间'
    // ...
  },
  yAxis: {
    type: 'value',
    name: '销售数量'
    // ...
  }
  // ...
};
```

在二维坐标中可以配置多个xy轴，但是需要通过offset属性来防止重叠

### 5.1 axisLine->配置轴线

例如两端的箭头等

```js
option = {
  xAxis: {
    axisLine: {
      symbol: 'arrow',
      lineStyle: {
        type: 'dashed'
        // ...
      }
    }
    // ...
  },
  yAxis: {
    axisLine: {
      symbol: 'arrow',
      lineStyle: {
        type: 'dashed'
        // ...
      }
    }
  }
  // ...
};
```

### 5.2axisTick ->配置刻度线

```js
option = {
  xAxis: {
    axisTick: {
      length: 6,
      lineStyle: {
        type: 'dashed'
        // ...
      }
    }
    // ...
  },
  yAxis: {
    axisTick: {
      length: 6,
      lineStyle: {
        type: 'dashed'
        // ...
      }
    }
  }
  // ...
};
```

## 5.3案例

```js
option = {
  tooltip: {
    trigger: 'axis',
    axisPointer: { type: 'cross' }
  },
  legend: {},
  xAxis: [
    {
      type: 'category',
      axisTick: {
        alignWithLabel: true
      },
      data: [
        '1月',
        '2月',
        '3月',
        '4月',
        '5月',
        '6月',
        '7月',
        '8月',
        '9月',
        '10月',
        '11月',
        '12月'
      ]
    }
  ],
  yAxis: [
    {
      type: 'value',
      name: '降水量',
      min: 0,
      max: 250,
      position: 'right',
      axisLabel: {
        formatter: '{value} ml'
      }
    },
    {
      type: 'value',
      name: '温度',
      min: 0,
      max: 25,
      position: 'left',
      axisLabel: {
        formatter: '{value} °C'
      }
    }
  ],
  series: [
    {
      name: '降水量',
      type: 'bar',
      yAxisIndex: 0,
      data: [6, 32, 70, 86, 68.7, 100.7, 125.6, 112.2, 78.7, 48.8, 36.0, 19.3]
    },
    {
      name: '温度',
      type: 'line',
      smooth: true,
      yAxisIndex: 1,
      data: [
        6.0,
        10.2,
        10.3,
        11.5,
        10.3,
        13.2,
        14.3,
        16.4,
        18.0,
        16.5,
        12.0,
        5.2
      ]
    }
  ]
};
```

## 6.视觉映射

暂时不想了解

## 7.图例-> legend属性

图例就是对图表中对内容区元素的注释，用不同的形状、颜色得令来表示不同的数据列，

## 8.事件与行为

echart的图表时刻监听用户的事件，主要分为两类：用户鼠标操作、交互

例如：点击图标后，就会跳转到百度

```js
// 基于准备好的dom，初始化ECharts实例
// var myChart = echarts.init(document.getElementById('main'));

// 指定图表的配置项和数据
var option = {
  xAxis: {
    data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
  },
  yAxis: {},
  series: [
    {
      name: '销量',
      type: 'bar',
      data: [5, 20, 36, 10, 10, 20]
    }
  ]
};
// 使用刚指定的配置项和数据显示图表。
myChart.setOption(option);
// 处理点击事件并且跳转到相应的百度搜索页面
myChart.on('click', function(params) {
  window.open('https://www.baidu.com/s?wd=' + encodeURIComponent(params.name));
});
```

