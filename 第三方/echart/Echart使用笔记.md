# Echart使用笔记

1. 快速入门

   * 使用三部曲

     1. 引入echart文件（大概也是js库）

     2. 创建带id和尺寸的容器dom

     3. 初始化实例

        1. 通过echart的init方法匹配id为main的dom   => myChart
        2. 根据图标api配置一套你想要的图标结构  =>option
        3. myChart.setOption(option)，通过setOptiong方法写入对象 

        ```html
        <body>
          <!-- 为 ECharts 准备一个定义了宽高的 DOM -->
          <div id="main" style="width: 600px;height:400px;"></div>
        </body>
        
        // 导入文件 
        <script src="echarts.js"></script>
        // 第二种导入文件的方式  import * as echarts form 'echarts.js'
        
        // 生成 eachart对象
        var myChart = echarts.init(document.getElementById('main'));
        
        // 配置对象
          var option = {
                title: {
                  text: 'ECharts 入门示例'
                },
                tooltip: {},
                legend: {
                  data: ['销量']
                },
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
        
        // 将配置写入对象
        myChart.setOption(option);
        
        
        ```

        

