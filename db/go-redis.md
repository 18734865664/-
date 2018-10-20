### go操作redis

#### 准备工作
1. 下载插件
	1.  go get github.com/gomodule/redigo/redis
2. 创建连接
	1. conn,err := redis.Dial("tcp", "127.0.0.1:6001")
3. [参考文档](https://godoc.org/github.com/garyburd/redigo/redis)

4. 类型转换
	1. go跟redis之间的交互使用四种基本类型，其他类型经过转换后传递
	2. []byte
	3. string
	4. int
	5. float64
	6. nil 转换为""空字符串
	7. true => "1", false=> "0"

#### 操作方式
1. 管道执行
	1. conn.Send(commond, arg...)
		1. 将命令发送至缓存区，不向redis发送
	2. conn.Flush()
		1. 将缓存区中命令，一次性全部发送给redis服务
	3. conn.Receive()
		1. 按照FIFO顺序依次读取服务器的响应
		2. 多次读取，会依次读出执行命令的返回值，如果调用receive多于send，会阻塞操作，等待。。。。。

2. 单独执行
	1. conn.Do(command, arg...) rel, error
	2. conn.String(rel)，将返回值强制转换为字符串
	3. 通常联合使用
	4. conn.String(conn.Do(command, arg...))
	5. 还有其他的类型转换函数

3. 返回值
	1. 常见类型
		1. redis.String
		2. redis.ByteSlices
		3. 提供了函数进行转换
	2. 符合类型接收
		1. 先使用redis.Values()接收
		2. 然后使用redis.Scan()接收
			1. Scan() 函数不支持接收自定义类型
![redis-接收复合类型.png](.\image\redis-接收复合类型.png)

	3. 自定义类型接收
		1. 通过对类型进行编码解码，进行存储，可以满足条件
		2. 可以转码成json或其他格式
