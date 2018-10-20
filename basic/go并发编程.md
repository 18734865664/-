Go 并发

	go在语言级别支持协程，叫goroutine。go语言标准库提供的所有系统调用操作（包括所有同步io操作），都会出让给CPU给其他goroutine。这让轻量级线程的切换管理不依赖于系统的线程和进程，也不需要依赖于CPU的核心数量
    
    
goroutine

		goroutine是Go并行设计的核心。goroutine说到底其实就是协程，它比线程更小，十几个goroutine可能体现在底层就是五六个线程，Go语言内部帮你实现了这些goroutine之间的内存共享。执行goroutine只需极少的栈内存(大概是4~5KB)，当然会根据相应的数据伸缩。也正因为如此，可同时运行成千上万个并发任务。goroutine比thread更易用、更高效、更轻便。
    
goroutine特性
1. main  goroutine结束，子goroutine随之结束
2. 注意这里：当main函数内部没有派生出子goroutine之前，只有进程的概念，
3. 当main函数派生出子goroutine之后，main降级为主goroutine，使用Goexit退出主goroutine，并不会将子goroutine杀死，
	1. 这种情况下，由于子goroutine调用后返回值的接收对象已经释放，会抛出runtime异常
	2. 

runtime.Gosched(): 
	出让当前goroutine所占用的cpu时间片
    调度器会安排其他等待的任务运行，并在下次再获得cpu时间轮片的时候，从该出让cpu的位置恢复执行
    
runtime.Goexit():
	退出当前goroutine
    Goexit之前定义的defer语句仍然生效
    
runtime.GOMAXPROCS():
	参数：要设置的使用核数
    返回值：设置使用的核数
    用途：设置当前进程使用的最大CPU核数
    
    
标准文件：
	当一个进程启动时，系统自动启动三个文件：
    	1. 标准输入 ----> stdin
    	2. 标准输出 ----> stdout
    	3. 标准错误 ----> stderr
    当程序结束时，系统自动回收
    
    
###channel:
channel是goroutine之间的通信机制，可以让一个goroutine发送值信息给另一个goroutine。

创建方式：

	var cha1 = make(chan type, n)
    cha1 := make(chan type, n)
    type 是channel传输的数据的类型
    n表示缓存区容量，缺省将初始化一个无缓冲区额channel
    
    

#### 特点：
1. channel跟map/slice类似，也是一个对应make创建的底层数据结构的引用，作为函数参数时，是引用传递
2. 默认值为nil
3. 同类型的channel可以使用==/!=进行比较，如果引用的对象相同，则channel相同，可以同nil比较
4. channel有两个行为：
	1. 发送   cha1 <- x
	2. 接收   x := <- cha1
	3. 允许自动推导类型，只有<-这一个操作符
	4. <- cha1  无接受变量的接收行为是允许的
5. channel支持close操作：
	1. 内置函数close(cha1)
	2. 任何基于已关闭channel的发送操作都会引起panic异常
	3. 允许对一个已经关闭的channel进行接收操作，
		1. 如果有已经发送进去的值，可以接收
		2. 如果没有数据，将产生一个零值数据
		
        
```
注意：在全局变量声明var cha1 chan int
不使用make进行初始化，没有分配内存区域，向channel中写入不报错，读数据时由于cha1值为nil，无数据可读，所以在这里会阻塞，导致程序无法向下执行
```

同一个goroutine中调用同一个channel的读写，会引起死锁

#### channel 分类
	无缓存channel拥有实现同步操作的能力
    有缓存channel拥有实现异步操作的能力

##### 不带缓存的channel

	无缓存的channel(unbufferd channel)：接受前没有能力保存任何值

1. 无缓存channel接收/发送双方有一方不执行操作，都会形成阻塞
2. 基于无缓存channel的发送和接收操作将导致两个goroutine做一次同步操作，所以无缓存channel也叫做同步channel

happens before：

	通过一个无缓存channel发送数据时，接收者收到数据发生在唤醒发送者goroutine之前
    happens before是go语言并发内存模型的一个关键术语
    
    
##### 带缓存的channel
    带缓存的channel内部持有一个元素队列（使用双向循环队列创建）
    队列的最大容量在调用make函数创造时通过第二个参数指定
    
读取数据：
1. 向缓存channel的发送操作就是向内部缓存队列的尾部插入元素
2. 接收操作则是从队列的头部删除元素
3. 如果内部队列是满的，发送操作将阻塞
4. 如果队列内时空的，读取操作将阻塞

带缓存的channel可以使用len(),cap() 函数

尽量避免使用channel作为goroutine中的队列使用，很容易发生阻塞，引起程序崩溃

    
    
#### 管道 --> 串联的channels

	一个channel的输出作为另一个channel的输入
    
#### 单方向channel
	当一个channel作为一个函数参数时，它一般总是被专门用于只发送或者只接收
    所谓的单项channel，其实只是对channel的一种使用限制
    channel本身必然是同时支持读写的
    
1. 只接收： chan <- int
2. 只发送： <- chan int
3. 这种限制在编译期进行检测
4. 任何双向channel向单向channel变量的赋值操作都将导致该隐式转换，但是没有反向转换的语法
	1. ch1 := make(chan int)
	2. ch2 := <- chan int(ch1)
    
#### channel的关闭/迭代
1. 没有办法直接测试一个channel是否被关闭
2. 类似map取值，可以返回两个值
	1. 第一个值是channel中取到的数据
	2. 第二个值是bool值，表示是否成功从channel中接收到数据
3. go语言range循环可以直接迭代channels
4. 关闭一个已经关闭的channel/为nil的channel将引发一个panic异常
5. 关闭一个channels的同时会触发一个广播机制
6. 一个关闭的channel，可以继续从中读取数据，读完缓存后，返回类型默认值。（与阻塞状态作区分）


#### select
1. unix中，通过调用select()函数监控一系列的文件句柄，一旦其中一个文件句柄发生了IO操作，该select()调用就会被返回，后来该机制也被用于实现高并发的Socket服务器程序
2. go语言直接在语言级别支持select关键字，用于处理异步IO
3. select语法类似switch
	1. 每个case语句里必须是一个IO操作
	2. select关键字后面不带判断条件
	3. 都可以使用break退出代码块
4. select 中如果多个case都可以执行，随机执行某一代码块
5. 与switch差别
	1. select 本身不带有循环性质，同时监听所有case
	2. 

![select语法.png](.\image\select语法.png)
	
    如果没有default语句，select代码块将阻塞至有一个case可以执行的时候。但可能出现出现忙轮询死循环
    所以一般不写default语句，阻塞后会挂起，释放cpu
    
select在go语言中经典用法:
可以实现超时机制，防止阻塞造成的程序终止运行：

![select_timeout.png](.\image\select_timeout.png)


#### 生产者消费者模型

	生产者：生产数据
	缓存区：
    	1. 解耦（降低生产者和消费者之间耦合度）
			低耦合高内聚
        2. 并发（生产能力和消费能力不对等时，保持正常运转）
        3. 缓存（生产者和消费者处理速度不一致时，暂存数据）
	消费者：消费数据
#### 同步
##### 同步锁（两种锁）
1. sync.Mutex
	1. 独占锁，当一个goroutine获得Mutex后，其他goroutine只能等待该锁释放
2. sync.RWMutex
	1. 单写多读模型
	2. 读锁（调用RLock()）占用的情况下，阻塞写操作，不阻塞读操作
	3. 写锁(调用Lock)会阻塞读写操作
	4. RWMutex类型中包含了Mutex
	5. 要保证锁释放（Unlock()或RUnlock())
编程中使用到的都是建议锁，强制锁只在操作系统层面


#### 三种定时方法
1. time.Timer定时器
	
    //// 创建定时器
	T := time.NewTimer(time.Second * 5)
	T.Stop()   // 停止定时器（将定时器归零）
    T.Reset(1 * time.Second)    //重置定时器
	// 定时到达后，系统自动向T.C 写入系统当前时间
	// 需要读取后，解除阻塞
	<- T.C   // 停止后，直接执行这一步会阻塞，因为channel写端被阻塞
    

![timer.png](.\image\timer.png)


2. time.Sleep()
3. time.After()
	1. 会返回一个time.Timer.C
4. time.NewTicker
	1. 系统周期性向结构体变量C（chan time）中写入当前系统时间
	2. 
![timeTicker.png](.\image\timeTicker.png)

#### 死锁
单goroutine死锁
	1. channel起码在两个以上的goroutine间通信
goroutine访问顺序导致死锁
多channel，多goroutine交叉死锁

	go语言中尽量不要将互斥锁和channel混用，会产生隐性死锁


#### 条件变量
sync.Cond

cond.wait:
1. 阻塞等待条件变量满足
2. 释放已掌握的互斥锁，相当于Cond.L.Unlock()
	1. 注意两步为一个原子操作
3. 当被唤醒，Wait()函数返回时，解除阻塞并重新获取互斥锁，相当于cond.L.Lock()

cond.Signal()
单发通知，给一个正等待（阻塞）在该条件变量上的goroutine发送通知

cond.Broadcast()
广播通知，给所有正在等待（阻塞）的goroutine发送通知，唤醒程序
	1. 会产生惊群效应
    2. 使用signal有可能在整体判断结束时，由于新唤醒程序再次进入休眠，形成死锁
    
![条件变量.png](.\image\条件变量.png)

######使用
1. 创建条件变量： var cond sync.Cond
2. 指定条件变量用的锁： cond.L = new(sync.Mutex)
3. cond.L.Lock() 给资源加锁
4. 判断是否达到阻断条件，达到使用cond.wait()



### 并发退出
两个问题：
1. 关闭的无缓存channel，可以从中读取channel传递类型的默认值，利用这个特性，定义个channel，接收输入值，关闭channel触发goroutine关闭动作
2. 利用sync.Waitgroup保证main goroutine作为最后一个关闭

![关闭goroutine.png](.\image\关闭goroutine.png)
