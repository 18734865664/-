# go web编程

## 1. 工作环境

### 1.1 GOPATH

> src  存放源码
>
> pkg  编译后生成的文件   执行go install  packageName, 会生成编译后的文件
>
> bin  编译后生成的可执行文件
>
> go build  会忽略“_", "."开头的文件

## 2. go 基础

> 25个系统关键字
>
> var和const参考2.2Go语言基础里面的变量和常量申明
> package和import已经有过短暂的接触
> func 用于定义函数和方法
> return 用于从函数返回
> defer 用于类似析构函数
> go 用于并行
> select 用于选择不同类型的通讯
> interface 用于定义接口，参考2.6小节
> struct 用于定义抽象数据类型，参考2.5小节
> break、case、continue、for、fallthrough、else、if、switch、goto、default这些参考2.3流程介绍里面
> chan用于channel通讯
> type用于声明自定义类型
> map用于声明map类型数据
> range用于读取slice、map、channel数据 
>
> 大写开头的变量/函数，是可以导入的  ps: 进行编码解码时很容易出错，定义为小写的变量无法被导出
>
> 数组赋值，是进行值传递，赋值传递副本
>
> Slice 不是真正意义上的数组，而是一个引用类型，指向一个底层数组
>
> New 返回指针   make返回初始化后的值
>
> defer   多次调用采用后进先出的模式
>
> 函数可以作为值、类型
>
> Panic和Recover:
>
>  	1. panic 发生时，会终止当前函数，并向调用方传递，一直到main终止
>  	2. Recover 可以让发生panic的goroutine恢复正常终止状态
>
> init函数：包调用时自动调用init函数（一个包中可以定义多个init,但不推荐）
>
> main: 初始化/执行的起始点，所以一个工程中如果多次调用了一个package，起始在main看，后面的几次调用并不会真的发生调用动作（也就是一个工程只会初始化一次）
>
> import 操作
>
> 1. 点操作
>    1. ​    .   "fmt"
>    2. 可以省略包名进行调用，Println("hello")
> 2. 别名操作
>    1. f  "fmt"      eg: f.Println()
>    2. 使用别名调用
> 3. 下划线操作
>    1. 调用了包的init函数，并不导入其中的函数
>
> go struct  通过匿名字段形式，实现字段继承
>
>  	1. 匿名字段可以是go中的struct/自定义type/内置类型
>  	2. 对重名字段，最外层的优先访问，继承字段，通过匿名变量访问
>  	3. 支持方法重写
>  	4. 
>
> 测试：
>
> 1. GoStub           go get -u -v github.com/prashantv/gostub
> 2. GoConvey      go get -v -u github.com/smartystreets/goconvey
> 3. GoMock         go get -u -v github.com/golang/mock/gomock
> 4. Monkey          go get -u -v github.com/bouk/monkey
>
> go 函数中的参数传递其实都是值传递，传递引用需要通过传递指针实现（go会自动获取其中的值直接返回）
>
> 接口（interface）：go中所有的类型都实现了空接口（interface{})
>
> 1.  go 通过interface实现了duck-typing:即"当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子"。
> 2. interface支持通过匿名字段继承
>
> select  可以监听channel上的数据流动

### 2.1 互联网基础

>DNS：
>
>1. 递归查询：逐级上报，直到查询到管理节点（到根节点还没有则返回失败）
>2. 迭代查询：直接上报至根节点，然后分发
>
>HTTP:
>
>1. http协议是无状态的，web引入了cookie机制维护连接的可持续状态
>
>2. 四种基本方法：GET/POST/DELETE/PUT
>
>3. HTTP协议
>
>   1. Request
>      1. request line
>         1. 请求方法  请求uri  http协议/协议版本
>      2. request header
>         1. Host:   xxx.xxxx.com
>         2. User-Agent: 
>         3. ....
>      3. request body
>         1. request body  与request header之间有空行（\n）作为分割
>         2. POST请求传递的参数
>   2. Response
>      1. status line (第一行)
>         1. http协议/版本号   状态码   状态消息
>      2. Response header
>         1. Server : nginx/1.0.8     服务器使用的web软件名及版本
>         2. Date:        发送时间
>         3. Content-Type: text/html   服务器发送信息的类型
>         4. Connection： keep-alive     保持长连接
>         5. Content-Length：响应主体长度
>      3. Response body
>         1. response body 与response header之间使用空行分隔（\n\n）
>
>4. http包执行流程(http.ListenAdnServe())
>
>   1. 创建listen Socket    --> 初始化一个server对象，然后调用了net.Listen()
>   2. Listen Socket接收客户端通信    -->    调用了前面获取的listener对象的Accept方法，每接收到一个请求，就调用一次Server.Serve方法（用一个goroutine）使用context进行上下文传递，并控制子goroutine的关闭，使用recover防止panic冒泡
>   3. 处理客户请求（路由到对应的handle）
>      1. 如果client请求的资源中有动态脚本，就调用对应的解释器处理后返回
>      2. listenAndServe 传入的第二个参数就是一个router，一般传入nil，使用默认的即可
>
>5. go  http 包
>
>   1. 核心功能
>
>      1. conn
>
>         1. go使用goroutines处理conn的读写事件，每个请求都保持独立，可以高效响应
>         2. 每次请求都会先创建一个conn，conn中保存了该次请求的信息，然后传递给对应的handle，handle从conn中读取request  header，这样保证每个请求的独立性
>
>      2. ServeMux(路由到handle)
>
>         1. >type ServeMux struct {
>            >
>            >​	mu sync.RWMutex     // 处理并发请求
>            >
>            >​	m  map[string]muxEntry      // 存放路由规则
>            >
>            >}
>            >
>            >type   muxEntry struct {
>            >
>            >​	explicit  bool   // 是否精确匹配
>            >
>            >​	h    handler       // 路由对应的处理函数
>            >
>            >}
>            >
>            >type  Handler  interface{
>            >
>            >​	ServeHTTP(ResponseWriter, *Request)    //  路由实现
>            >
>            >}      // 调用handlerFunc   会将传入的函数转换为Serve'HTTP()
>
>         2. 自定义ServeMux,    只要实现了ServeHTTP方法，在里面实现自定义的分发逻辑即可
>
>   2. 
>
>Web工作方式的几个概念：
>
>1. Request
>2. Response
>3. Conn：用户的每次请求链接
>4. Handler: 处理请求和生成返回信息的处理逻辑
>
>

## 3 表单处理

> 1. 由于不能信任任何用户的输入，所以我们需要对这些输入进行有效
>    1. 一方面可能会引起运行错误
>    2. 另一方面可能是客户端的恶意攻击

### 3.1 处理表单输入

> 1. 需要显式的调用r.ParseForm() 解析form表单
>
>    1. 默认不会进行解析的，所以r.Form["username"]无法获取数据，这样获取的是slice
>    2. 使用r.FormValue("username")   会自动调用ParseForm，获取的是string
>

### 3.2 表单验证

#### 3.2.1 必填字段

> 1. 通过r.Form["username"]获取，如果是不存在的字段，返回nil
> 2. 通过r.Form.Get()   获取，不存在的字段会返回空值，但是只能获取单个值，map还是要使用第一种方式

#### 3.2.2 数字

> 1. 使用类型转换，进行判断
> 2. 使用正则表达式，进行匹配

#### 3.2.3 中文

> 目前只能使用正则形式判断：[\\x{4e00} - \\x{9fa5}]

#### 3.2.4 英文

> 正则匹配：[a-zA-Z]

#### 3.2.5 email

> ^([\w\.\_]{2,10})@(\w{1,}).([a-z]{2,4})$

#### 3.2.6 手机号

> (1\[3|4|5|8][0-9]\d{4,8})

#### 3.2.7 下拉框/单选按钮

> 使用初始化一个slice，对slice进行遍历匹配，防止恶意伪造不存在的选项发送值

#### 3.2.8 复选框

> 方法与下拉框类似，区别在于返回的数据是个slice，同样需要对其进行遍历

#### 3.2.9 日期和时间

> 使用time.Date方法，转换后进行判断

#### 3.2.10 身份证号码

> 15未和18位两种，18位身份证号，最后一位可能是X
>
> ^(\d{15})$
>
> ^(\d{17})([0-9]|X)$

### 3.3 预防跨站

> 动态站点会受到一种”跨站脚本攻击“（Cross  Site  Scripting,XSS）,静态站点不受此攻击影响
>
> 两种手段：
>
> 1. 对所有输入参数都进行验证
>
> 2. html/template 中的转义函数
>
>    1. func HTMLEscape(w io.Writer, b []byte) //把b进行转义之后写到w
>    2. func HTMLEscapeString(s string) string //转义s之后返回结果字符串
>    3. func HTMLEscaper(args ...interface{}) string //支持多个参数一起转义，返回结果字符串
>    4. 上面三个函数返回显示的是转义后的内容，写入到html模板中，显示输入的纯文本
>

### 3.4 防止多次提交

> 1. 在请求表单的时候，加入一个依据unix时间戳计算得到的token，提交时，校验token有效性即可
> 2. 也可以通过script事件，再提交后，禁用提交操作

### 3.5 处理文件上传

> 一定要设置form的enctype属性
>
> 1. application/x-www-form-urlencoded
>    1. 默认，表示发送前编码所有字符
> 2. multipart/form-data   
>    1. 不对字符编码（）
> 3. text/plain
>    1. 空格转化为”+“， 但不对特殊字符编码
>
> 三个步骤：
>
> 1. 表单中增加enctype="multipart/form-data"
> 2. 服务端调用r.ParseMultipartForm，传入一个内存存储最大值（例如32 << 20  表示32kb），超出部分存放在服务器本地的缓冲中
> 3. 调用r.FormFile获取文件句柄，使用io.copy进行文件流之间传递

## 4 访问数据库

### 4.1 database/sql接口

> go本身没有提供数据库驱动，只是提供了一系列的标准接口，开发者根据接口进行驱动开发
>
> 1. sql.Register
>
>    1. 注册数据库驱动driver
>
> 2. driver.Driver
>
>    1. 返回一个数据库conn接口
>    2. 这个Conn只能用于一个goroutine，不能多个goroutine公用
>
>       3. dirver.Conn
>                1. 只能用于一个goroutine
>    4. driver.Stmt
>          1. 是一种准备好的状态，与Conn相关联
>    5. Driver.Tx 
>          1. 事务，有commit/rollback两个函数
>    6. driver.Execer
>    7. driver.Result
>    8. driver.Rows
>          1. 返回查询的接口集
>    9. driver.Value
>          1. 一个空接口，可以放任何数据
>    10. driver.ValueConverter
>           1. 定义了如何把普通值转化为driver.Value的方法
>    11. dirver.Valuer
>           1. 返回driver.Value的方法
>    12. database/sql ，
>    13. 在database/sql/driver 接口之上进行了整合，内部实现了一个conn pool

### 4.2 mysql

> 连接：db1, err := sql.Open("mysql", "root:123123@tcp(127.0.0.1:3306)/test?charset=utf8")
>
> 预编译语句：
>
> 1. db.Prepare("INSERT userinfo set username=?,departname=?,created=?")
> 2. 比手动拼接sql高效
> 3. 可以防止sql注入攻击
> 4. 一般利用prepare 和exec 完成insert/update/delete操作，
>
> 查询：
>
> 1. 调用db.Query执行sql语句，会返回一个Rows结果集
> 2. 通过rows.Next()迭代查询结果
> 3. 通过rows.Scan读取每一行值
> 4. 调用db.Close关闭查询

### 4.3 redis

> go get github.com/garyburd/redigo/redis
>
> redis.pool   创建连接池
>
> redis.Do    执行命令



## 5 session 和数据存储

### 5.1 cookie

> 一种客户端技术，保存在客户端本地，所以存在一定的风险（会被人获取）
>
> 根据生命周期，cookie分为两种
>
> 1. 会话cookie
>    1. 随浏览器关闭，而失效，保存在内存中
> 2. 持久cookie
>    1. 保存在本地磁盘，有失效时间
>
> go 设置/读取cookie
>
> http.Cookie   一个结构体
>
> http.SetCookie(w, &cookie)
>
> r.Cookie(key)      //读取cookie
>
> for _, cookie := range r.Cookies()

### 5.2 session

> session 隐含了“面向连接”和“保持状态”两个含义
>
> 一种服务端机制，是一个类似散列表的结构保存信息
>
> 客户端和服务端依靠一个全局唯一的标识访问这份数据
>
> session创建过程：
>
> 1. 生成全局唯一sessionId
> 2. 开辟存储空间（默认保存在内存中），为了会话的安全和共享，将其存放到数据库中
> 3. 将sessionId发送会客户端
>
> 返回session
>
> 1. Cooike
> 2. url重写 --- 返回给用户的页面里，所有url都追加sessionId标识，从而实现会话保持，在客户端禁用session时，仍有效、
>
> web开发中，就四个操作：设置值，读取值，删除值，获取当前sessionID
>
> Session劫持： 利用用户正常登陆后的session，模拟用户登陆
>
> 1. cookieonly: cookie中的一个属性，设置为true，则不能通过前端脚本（js)操作cookie
>    1. 可以防止cookie被XSS读取
>    2. cookie不会像URL重置的方式那么容易获取SessionId
> 2. 每一次访问都设置一个新的token，下一次请求校验该token，如果不一致，则跳转登陆
> 3. 间隔生成新的SID
>    1. 为session设置一个单独字段，保存生成时间，超过事件就重新生成一个sessionid返回
>    2. **sess1 :=testSession.GSessionMSG.SessionRegenerateID(w,r)**
>       1. 重新生成ID，session内容仍然可以获取
> 4. 

## 6. 文件处理

### 6.1 XML

> XML本质上是一种树形的数据格式，而我们可以定义与之匹配的go 语言的 struct类型，然后通过xml.Unmarshal来将xml中的数据解析成对应的struct对象
>
> v := testXml.Recurlyservers{}
> err = xml.Unmarshal(data, &v)   // Unmarshal   采用reflect辅助获取数据的
>
> xml中是大小写敏感的，所以创建字段时要一一对应
>
>  	1. 这里的一一对应，指的是创建struct时，tag中的内容
>  	2. reflect 解析时，先匹配struct  tag，如果没有，则去匹配字段名

>
>
>XMLName不会被输出
>
>tag中含有"-"的字段不会输出
>tag中含有"name,attr"，会以name作为属性名，字段值作为值输出这个XML元素的属性，如上version字段所描述
>
>tag中含有",attr"，会以这个struct的字段名作为属性名输出为XML元素的属性，类似上一条，只是这个name默认是字段名了。
>tag中含有",chardata"，输出为xml的 character data而非element。
>tag中含有",innerxml"，将会被原样输出，而不会进行常规的编码过程
>tag中含有",comment"，将被当作xml注释来输出，而不会进行常规的编码过程，字段值中不能含有"--"字符串
>
>tag中含有"omitempty",如果该字段的值为空值那么该字段就不会被输出到XML，空值包括：false、0、nil指针或nil接口，任何长度为0array, slice, map或者string
>tag中含有"a>b>c"，那么就会循环输出三个元素a包含b，b包含c，例如如下代码就会输出 
>FirstName string   `xml:"name>first"`
>LastName  string   `xml:"name>last"`
><name>
><first>Asta</first>
><last>Xie</last>
></name>

### 6.2 JSON

> JSON与XML最大的不同在于XML是一个完整的标记语言，而JSON不是。JSON由于比XML更小、更快，更易解析,以及浏览器的内建快速解析支持,使得其更适用于网络数据传输领域
>
> 查找Foo字段：
>
> 1. 首先查找tag含有Foo的可导出的struct字段(首字母大写)
> 2. 其次查找字段名是Foo的导出字段
> 3. 最后查找类似FOO或者FoO这样的除了首字母之外其他大小写不敏感的导出字段 

### 6.3 template

> go 模板语言使用{{}}   进行占位 {{.}}   点号标识传入的对象
>
> 注意：会报错，因为我们调用了一个未导出的字段，但是如果我们调用了一不存在的字段是不会报错的，而是输出为空。
>
> 循环输出：使用{{range ...}}{{end}}或{{with ojb}}{{end}}
>
> with是指操作obj对象，类似于上下文的概念
>
> 判断语句：{{if}}  {{else}} {{end}}





## 7 通信

### 7.1 Socket编程

>常见socket有两种：流式SOCK STREAM，数据报式Socket（SOCKDGRAM）
>
>利用tcp/ip协议簇进行网络标识，三元组（IP地址，协议，端口）

### 7.2 webSocket

> HTML5的重要特性，实现了基于浏览器的远程socket，使浏览器和服务器可以进行全双工通信
>
> 相比传统HTTP：
>
> 1. 一个Web客户端只建立一个TCP连接
> 2. Websocket服务端可以推送(push)数据到web客户端.
> 3. 有更加轻量级的头，减少数据传送量
>
> webSocket Url 起始输入是 ws://   wss://
>
> WebSocket  建立连接后，通讯数据都是以“\x00“开头，以"\xFF"结尾，浏览器会自动”掐头去尾“。

### 7.3 REST

> 什么是RESTful
>
> 1. 每一个URI代表一种资源
> 2. 客户端和服务器之间，传递这种资源的某种表现层
> 3. 客户端通过四个HTTP动词，对服务器资源进行操作（post/put/get/delete）,实现”表现层状态转化"
>
> REST原则：
>
> 1. 无状态
>    1. 每次请求都带有所有需要的信息
>    2. 服务端可以在任何时候重启，且不会通知客户端
>    3. 客户端可以缓存数据，改进性能
> 2. 系统分层
>    1. 组件无法了解本层之外的组件
>
> 架构风格，汲取了WWW的成功经验：无状态，以资源为中心，充分利用HTTP协议和URI协议，提供统一的接口定义

### 7.4 RPC

> socket/http是“信息交换”的模式提供服务，客户端发送信息到服务端，服务端回应一定的信息
>
> Rpc是实现函数的远程调用，客户端像调用本地函数一样，然后把这些参数打包后发送到服务端，服务端解包到相应函数进行处理，然后将结果返回个客户端
>
> RPC服务跨越了传输层和应用层
>
> go 官方实现的RPC服务只支持go程序之间交互，所以采用了Gob编码，支持http/tcp/jsonrpc三个级别
>
> Go RPC要求：
>
> 1. 函数必须是可导出的（首字母大写）
> 2. 必须有两个导出类型的参数
> 3. 第一个参数是接收的参数，第二个参数是返回给客户端的参数，第二个参数必须是指针类型
> 4. 函数还要有一个返回值error
>
> func (t *T) MethodName(argType T1, replyType *T2) error

> Jsonrpc    目前只支持tcp模式的网络传输，不能使用http，与gorpc相比，只是编码方式变为了json，而不是gob，其他与gorpc一样

# 8 安全与加密

## 8.1 预防CSRF攻击

> CSRF(Cross-site  request  forgery)跨站请求攻击，也叫XSRF
>
> 攻击条件：（缺一不可）
>
> 1. 登陆受信任网站A，并生成本地Cookie
> 2. 不退出A的情况下，访问了危险链接B
>
> CSRF 主要利用了Web的隐式身份验证机制，web身份验证机制可以保证请求是源于用户的浏览器，却无法保证是用户主动发出的
>
> 预防CSRF：（可以在服务端/客户端进行，一般在服务端进行防御）
>
> 1. 正确使用GET，POST，Cookie
>    1. 类似restful的形式，不同方法实现不同功能，比如GET只用于获取资源，POST只用于提交修改
> 2. 非GET请求中增加伪随机数
>    1. 为每个客户生成单独的cookie  token，所有表单提交都携带这个token，理论上恶意网站无法获取第三方的cookie，所以可以防止CSRF。如果存在XSS漏洞被利用，用户Cookie就可能被窃取
>    2. 每个请求都是用验证码，完美方案，体验极差，不可取
>    3. 每次请求后，生成随机数，页面中表单增加该token（隐藏），提交后，验证token值。

## 8.2 确保输入过滤

> 1. 识别数据
>    1. get/post方法提交的数据很好确认，其实header中的很多元素也是可以用户自定义的，也需要进行验证
> 2. 过滤数据
>    1. 过滤的过程更应该视为一个检查的过程，尽量不要试图修改输入的不合法数据，当使用过滤规则造成误判时，增加白名单机制
>    2. 使用strconv包提供的方法进行格式转换（Parse系列/
>    3. string 包下面的trim/toLower等函数
>    4. regexp包处理email等复杂的需求
> 3. 区分已过滤及被污染数据，如果存在攻击数据就保证过滤之后可以提供更安全的数据
>    1. 可以为每个连接，初始化一个全局变量，存放过滤后的干净数据

## 8.3 避免XSS攻击

> Cross Site Scripting )跨站脚本攻击 ---- 只有动态站会受影响
>
> 分类：
>
> 1. 存储型XSS
>    1. 如评论/文章等，存入后端数据库，显示到客户端后，用户浏览就会触发脚本
> 2. 反射型XSS
>    1. 主要做法是将脚本代码加入URL的请求参数中，请求参数进入程序后在页面直接输出，用户点击连接就可能受攻击
>
> 危害：
>
> 1. 盗用cookie，获取敏感信息
> 2. 利用植入的flash，通过crossdomain权限设置进一步获取更高权限
> 3. 利用iframe/fram/XMLHttpRequest或上述Flash等方式，以（被攻击者）用户的身份执行一些管理动作，或执行一些操作（如新浪遭受的XSS攻击）
> 4. 利用可被攻击的域受到其他域的信任，以受信任来源的身份请求一些平时不允许的操作
> 5. 在访问量大的页面上XSS可以攻击一些小型网站，实现DDOS攻击
>
> 预防XSS
>
> 1. 不相信用户的任何输入，过滤掉输入中的所有特殊字符
> 2. 指定HTTP头部类型
>    1. w.Header().Set("Content-Type","text/javascript")
>    2. 可以让浏览器只解析js代码，而不是html的输出

## 8.4 避免SQL攻击

> SQL Injection）注入攻击，攻击者将非法sql字段传递给后台服务器，后台服务其实就是数据加工的过程，变量替换后，会执行一个攻击者所希望执行的sql命令
>
> 防护：
>
> 1. 严格限制web应用的数据库操作权限
> 2. 检查输入的数据，是否符合预期
> 3. 对入库数据进行转义
> 4. 不要在项目中直接拼接sql语句，使类似orm之类的数据库操作框架
> 5. 发布前使用SQL注入检查工具检测（sqlmap,sqlninja)
> 6. 自定义错误页面，不直接将错误暴露给前台，防止攻击者获取信息
>
>

## 8.5 存储密码

> 不存放明文和单纯的hash摘要
>
> 将用户名，用户密码，加盐（salt）后获取散列摘要，入库，保证每个用户都有不同的密码值

## 8.6 加密与解密数据

> 简单加解密   base64
>
> 高级加解密：aes/des

# 9 错误处理

> go中采用与C语言类似的判断错误返回值的方式处理异常，所以自定义错误检查函数，在项目中大量服用，极大的减少了代码的冗余

## 10 Gdb 调试

> go build -gcflags "-N -l" main.go
>
> gdb main
>
> run   开始执行代码
>
> b (bread)   10      --> 在第10行打断点
>
> l  （list)        --> 输出源代码，默认10行
>
> d   (delete)          --> 删除断点
>
> ​	info breakpoints    显示断点信息
>
> bt  (backtrace)         --> 打印执行的代码过程
>
> info    --> 用来显示参数
>
> ​	info  locals     -->     显示当前执行的程序中的变量值
>
> ​	info   breakPoints       --> 显示断点列表
>
> ​	info    goroutines        -->  显示当前执行的goroutine列表
>
> p  (print)            -->  用来打印变量或其他信息
>
> whatis                -->   显示变量类型
>
> n  (next)             -->  单步调试，跳到下一步
>
> c (coutinue)      --> 跳出当前断点，后面跟N标识跳过多少个断点
>
> set    variable     --> 改变运行过程中的变量值

## 11 后期维护

### 11.1 日志服务

> seelog
>
> beego/log

### 11.2 网站错误

#### 11.2.1 错误类型

> 1. 数据库错误
>    1. 连接错误
>    2. 查询错误
>    3. 数据错误
> 2. 应用运行时错误
>    1. 文件系统和权限
>       1. 读取不存在的文件等
>    2. 第三方应用调用错误
> 3. HTTP错误：
> 4. 操作系统错误
>    1. 磁盘满了/资源耗尽
> 5. 网络出错
>    1. 用户请求中断
>    2. 读取数据中断（超时）

#### 11.2.2 错误处理目标

> 1. 通知访问用户出现错误
>    1. 返回错误信息给用户
> 2. 记录错误
> 3. 回滚当前的操作请求
> 4. 保证现有程序可运行客服务
>    1. recover机制

## 12 如何设计一个web框架

### 12.1 项目规划

![mvc](H:\go\markdown\goweb编程\img\mvc.png)

### 12.2 自定义路由器设计

### 12.3 controller

### 12.4 日志和配置设计

### 12.5 案例

