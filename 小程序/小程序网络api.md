# 小程序网络api

1. 上传

   1. wx.uploadFile(Object object)

      将本地资源上传到服务器,客户端发起一个https post请求 其中content-type为multipart/form-data

   2. 参数:Object object

      * url:	string     是       开发者的服务器地址

      * filePath  string  是    要上传文件

      * name      string   是   文件对应的key,开发者在服务端可以通过这个key获取文件的二进制内容

      * header    object  否  http 请求头

      * success \fail\complete

        * success(data,statuCode)成功地数据和状态码

        * data:UploadTask(一个可以监听上传进度变化的事件和取消上传的对象)

        * wx.chooseImage选择本地图片或拍照

          ```javascript
          wx.chooseImage({
          	success(res) {
          		const tempFilePaths = res.tempFilePaths
          		wx.uploadFile({
          			url:'https://example.weixin.qq.com/upload',
          			filePath:tempFilePath[0],
          			name:'file',
          			formData:{
          				'user':'test'
          			},
          			success(res){
          				const data = res.data
          			}
          		})
          	}
          })
          ```

        * data:UploadTesk:这是一个可以监听上传进度的事件,以及取消上传任务的对象

        * 方法

          * 中断上传任务
          * 监听上传进度

2. 下载

   wx.downloadFile

   下载资源到本地,客户端直接发起一个https,Get请求,返回文件的本地临时路径(本地路径)

   单词下载允许最大文件为200mb

   注意:请在服务端响应

   * 参数

     * url        string     是     下载资源的url
     * header    object  否      http请求的header
     * filePath   string    否   制定文件下载后的储存路径(本地路径)
     * success
       * tempFilePath       string     临时文件路径(本地路径)没传入filepath路径时回返回
       * filePath      string    用户文件路径  传入filepath时回返回
       * statusCode   number  开发者服务器返回的http状态码
       * profile   object  网络请求中一些调试信息
         * profile 结构信息.....省略

   * 示例代码

     ```{}
     wx.downloadFile({
     	url:'https://example.com/audio/123'
     	surress(res){
     		// 只要服务器有响应数据,就会把响应内容写入到success回调中
     		if (res.statusCode === 200) {
     			wx.playVoice(){
     				filePath:res.tempFilePath
     			}
     		}
     	}
     })
     ```

     

   

   

