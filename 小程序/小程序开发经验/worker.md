# worker( 线程)

js是单线程

1. 有些密集计算或高延迟的场景，就不太行
2. 电脑都是多核的，无法充分发挥电脑的性能
3. worker的诞生

## worker说明

1. 主线程和worker线程之间是通过消息通信的，主线程不能直接调用worder线程中的函数
2. worder线程不能使用wx系列的api

## worker使用步骤

1. 配置

   * 在app.json中配置worder目录
   * ![2019715104847049.jpg](https://img.jbzj.com/file_images/article/201907/2019715104847049.jpg?2019615104857)

2. 主线程中创建调用和销毁

   * 注意:创建时填写的是绝对路径

     ```
     // wecome.js
     onload:funciton (){
     	const worker = wx.createWorker('/worker/myworker.js')
     	worker.postMessage({
     		x : 10,
     		y : 2
     	})
     	worker.postMessage({
     		console.log('这是主线程打印的')
     		console.log(res)
     	})
     }
     ```

3. worder线程中实现

   ```
   // myworker.js
   worker.onMessage(function(res){
   	console.log('这是worder内部线程打印的')
   	console.log(res)
   	let sum =  add(res.x,res.y)
   	worder.postMessage({
   		sum : sum
   	})
   	function add(x,y) {
   		return x+y
   	}
   })
   ```

    最终打印如下:

   ![2019715105513752.jpg](https://img.jbzj.com/file_images/article/201907/2019715105513752.jpg?2019615105522)

