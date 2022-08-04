# Echarts文档笔记

## 1、基础部分

* 安装方式（GitHub、npm、CDN，在线定制）
  * `npm install echart --save`
  * 在限定制：如果想只引入部分模块，以减少包体积



* 在项目中引入Echart

  * 引入Echart
  * js获取dom配置option

  ```js
  import * as echarts from 'echarts';
  
  // 基于准备好的dom，初始化echarts实例
  var myChart = echarts.init(document.getElementById('main'));
  
  // 绘制图表
  myChart.setOption({
    title: {
      text: 'ECharts 入门示例'
    },
    tooltip: {},
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
  });
  ```

  以下是按需引入的方式

  ```js
  // 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
  import * as echarts from 'echarts/core';
  // 引入柱状图图表，图表后缀都为 Chart
  import { BarChart } from 'echarts/charts';
  // 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
  import {
    TitleComponent,
    TooltipComponent,
    GridComponent,
    DatasetComponent,
    DatasetComponentOption,
    TransformComponent
  } from 'echarts/components';
  // 标签自动布局，全局过渡动画等特性
  import { LabelLayout, UniversalTransition } from 'echarts/features';
  // 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
  import { CanvasRenderer } from 'echarts/renderers';
  
  // 注册必须的组件
  echarts.use([
    TitleComponent,
    TooltipComponent,
    GridComponent,
    DatasetComponent,
    TransformComponent,
    BarChart,
    LabelLayout,
    UniversalTransition,
    CanvasRenderer
  ]);
  
  // 接下来的使用就跟之前一样，初始化图表，设置配置项
  var myChart = echarts.init(document.getElementById('main'));
  myChart.setOption({
    // ...
  });
  ```

  