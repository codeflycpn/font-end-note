# Echart各项参数说明

1. title：标题

   ```js
   	  title: {
   			    text: 'ECharts 入门示例'
   			  }
   ```

   

2. tooltip：工具栏

3. legend：顶部的统计，与series属性关联，写在data数组中

   ```js
   	  legend: {
   			    data: ['销量s','销量y','销量z'],
   			  },
   ```

   

4. xAsix：x轴显示的数据

   ```js
   	  xAxis: {
   			    data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
   			  },
   ```

   

5. yAsix：y轴显示的数据

6. series：配置对比的数据组

   ```js
   			  series: [
   			    {
   			      name: '销量s',
   			      type: 'bar',
   			      data: [5, 20, 36, 10, 10, 20]
   			    },
   				{
   				  name: '销量y',
   				  type: 'bar',
   				  data: [15, 120, 136, 110, 110, 120]
   				},
   				{
   				  name: '销量z',
   				  type: 'bar',
   				  data: [115, 121, 126, 111, 11, 130]
   				},
   			  ]
   ```

   