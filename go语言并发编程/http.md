### http编程
#### 请求报文结构


![http请求包.png](.\image\http请求包.png)


1. 请求行：请求方法 空格 请求文件URL 空格 协议版本 （\r\n）
2. 请求头：语法格式：key：value
3. 空行：\r\n  -- 代表http请求头结束
	1. 请求头部和请求包体之间的空格不能省略，必须是"\r\n"，http协议中的回车换行，都是使用”\r\n“

4. 请求包体： 请求方法对应的数据内容


#### 响应包格式

![http响应包.png](.\image\http响应包.png)
1. 状态行
2. 响应头部
3. 

#### 创建web服务器
1. 使用net/http包
	1. 注册回调函数
		1. http.HandleFunc("path", handler)
		2. 这里的path，是一种路由信息，客户端请求进来后，匹配到这里的path，交给handler负责解析，如果没有匹配到，则交给默认的系统的handler（ListenAndServe中指定的）
		2. 需要定义handler函数
			1. 函数形参列表必须是（w http.ResponseWriter, r *http.Request）
			2. w 是返回客户端的数据
			3. r是从客户端传入的数据
			
	2. 绑定服务器监听地址
		1. http.ListenAndServe("127.0.0.1:8080", nil)
        2. 第二个参数也是一个handler，传入nil会调用系统默认的一个handler


#### 创建客户端
1. 使用http.Get方法
	1. func Get(url string) (resp *Response, err error)
	2. resp中的Body是服务器应答包体，属于流式文件对象，需要手动执行关闭动作，关闭body数据流是请求方的义务
	3. resp本身不需要关闭，resp的Close成员，是个bool值，表示向对端发送请求后，是否关闭连接
2. 使用读取文件的方式获取请求返回值
	1. 注意这里的错误处理有点不同，读到最后一次的时候，同时将io.EOF返回
