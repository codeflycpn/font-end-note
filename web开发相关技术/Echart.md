# Echart

简介：使用JavaScript实现的开源可视化库，涵盖各行业图表，满足各种需求

## 1、安装

## 2、配置语法

1. 首先你要有html页面

2. 准备一个Echart的容器，要具备宽高

3. 添加配置信息

   Echarts库使用json格式来配置

   `echarts.init(document,getElementByid('main').setOption(option))`

   option：json数据个的配置文件，用于绘制制图表（参考官方文档）

   通过这段语句来控制dom，为其注入JavaScript针对Echart的配置

   ```html
   
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8">
       
       <!-- 引入 ECharts 文件 -->
       <script src="echarts.min.js"></script>
       
       <!--准备一个具备宽高的Dom-->
       <div id='main' style='width:600px;height:500px'></div> 
       
   </head>
   </html>
   
   ```

   以下为完整实例

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8">
       <title>第一个 ECharts 实例</title>
       <!-- 引入 echarts.js -->
       <script src="https://cdn.staticfile.org/echarts/4.3.0/echarts.min.js"></script>
   </head>
   <body>
       <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
       <div id="main" style="width: 600px;height:400px;"></div>
       <script type="text/javascript">
           // 基于准备好的dom，初始化echarts实例
           var myChart = echarts.init(document.getElementById('main'));
    
           // 指定图表的配置项和数据
           var option = {
               title: {
                   text: '第一个 ECharts 实例'
               },
               tooltip: {},
               legend: {
                   data:['销量']
               },
               xAxis: {
                   data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
               },
               yAxis: {},
               series: [{
                   name: '销量',
                   type: 'bar',
                   data: [5, 20, 36, 10, 10, 20]
               }]
           };
    
           // 使用刚指定的配置项和数据显示图表。
           myChart.setOption(option);
       </script>
   </body>
   </html>
   ```

   