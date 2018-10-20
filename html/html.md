### html（hyper mark-up language）
#### 简述
1. 标签分类
	1. 块级元素（行元素）
		1. 默认独占一行
		2. 宽度默认父级宽度
	2. 内联元素（行内元素）
		1. 元素之间可以排列在一行
		2. 宽高由内容撑开
		3. 元素之间默认有小间距
		4. 基准对齐（文字底部对齐）
		5. 内联元素在同一行写，则之间没有间距，如果换行写，则有默认的间距

2. 大小写
	1. html大小写不敏感，推荐使用小写

3. head
	1. title标签，定义了HTML文档的标题
		1. 是必须字段
	2. base： 定义了所有链接的默认目标url
		1. url不加协议头，则默认在现有url后进行拼接追加
	3. meta：提供了html文档的描述，关键词，作者，字符集。
		1. &lt;meta name = "keywords" content="HTML,CSS,XML" &gt;
		2. &lt;meta charset = "utf-8" &gt;
	4. style: 嵌入式css位置
		1. <pre>&lt;style type = "test/css"&gt;
				p {
                	background:blue;
                }
           &lt;/style&gt;
           </pre>
	5. script：js
	6. link： 外部引用css文件
		1. &lt;link rel="stylesheet" type="text/css" href="styles.css" &gt;
	7. noscript
4. MVC设计模型（Model View Controller)
	1. 将业务逻辑、数据、界面显示分离的方式组织代码
	2. Model：应用程序中用于处理应用程序数据逻辑的部分
		1. 通常模型对象负责数据库中存取数据
	3. view：处理数据显示的部分
		1. 通常用于依据模型数据创建
	4. controller：处理用户交互的部分
		1. 通常负责从视图读取数据，控制用户输入，并向模型发送数据


#### 块级元素

	默认情况下，html会自动在块级元素前后添加一个额外的空行
    
1. 标题标签 h1-h6
	1. 标题标签最好只用于标题，不要为了加粗或设定字号滥用
	2. 搜索引擎使用标题标签，结构化网页，编制索引
2. 段落标签 p
3. 通用容器标签 div
	1. 表示文档中一块内容，具有块级元素默认特质，没有其他默认样式
	2. 与p标签区分，没有默认的间距，外框宽度
	
#### 内联元素

<pre><strong>	内联元素之间默认有小空格，
	对齐方式为底部对齐</strong></pre>

1. 超链接标签 a， 也叫anchor(锚点)
	1. 设置缺省链接地址
		<pre>&lt;a href="#"&gt; &lt;/a&gt;
        #表示空链接</pre>
    2. target属性
    	1. <pre>&lt;a href="www.baidu.com" target="_blank"&gt; &lt;/a&gt;
        target = "_blank"后，链接会在新窗口中打开</pre>
    3. herf后面的网址，uri中的/一定不要省略，如果省略最后的斜杠，而请求的是一个目录，会向服务器发送两次请求
    4. 使用锚点实现超链（可以跨页面）
    	1. 锚点出通过id进行注册标识
    	2. &lt;a id = "testId"&gt;test&lt;/a&gt;
    	3. 通过"#"实现锚点跳转
    	4. &lt;a herf = uri#testId&gt;&lt;/a&gt;
    5. uri需要转码
2. 通用内联容器标签 span
3. 图片标签 img
	1. &lg;img
4. table
	1. tr 表示行
		1. rowspan = 2, 跨两列
	2. th 标识标题栏
	3. td 标识列
		1. colspan = 2， 跨两列
	4. border = 1外框为1px
	5. cellspacing = "10" 单元格之间间距
	5. align = "right"|"left"|"center"

5. 序列
	1. 无序列表
		1. &lt;ul&gt;&lt;/ul&gt;  无序列表标识
			1. style="list-style-type:square" 实心方点
			2. style="list-style-type:circle"  空心原点
			3. style="list-style-type:disc" 实心圆点
			4. style="list-style:none" 去掉圆点
		2. &lt;li&gt;&lt;/li&gt;  无序列表列表项
	2. 有序列表
		1. &lt;ol&gt;&lt;/ol&gt;   有序列表标识
			1. type = "A"|"a"|"I"|"i"
		2. &lt;li&gt;&lt;/li&gt;    列表项
	3. 自定义列表
		1. &lt;dl&gt;&lt;/dl&gt; 自定义列表标识
		2. &lt;dt&gt;&lt;/dt&gt;  每个自定义列表项起始标识
		3. &lt;dd&gt;&lt;/dd&gt; 自定义项标识

6. iframe
	1. 在当前页面中嵌入另一个页面
	2. 
![iframe.png](.\img\iframe.png)




#### 功能标签
1. 换行标签  &lt;br&gt;
2. 注释   <!-- -->
	1. 浏览器解释时忽略
	
3. & nbsp;  空格标签
	1. 不是很精确，精确的缩进使用样式实现
4. & lt; <
5. & gt; >
6. 预编译  &lt;pre&gt;&lt;/pre&gt;
	1. 预格式化文本，其中的内容保留换行符和空格
	2. 文件呈现等宽
	3. 可以导致段落断开的标签<b><big>不能</big></b>包含在里面
	
#### 格式化标签
6. &lt;b&gt;&lt;b/&gt;   加粗文本
7. &lt;i&gt;&lt;i/&gt;   斜体文本
8. &lt;sub&gt;&lt;sub/&gt; 下标
9. &lt;sup&gt;&lt;sup&gt; 上标
10. &lt;hr&gt;   分割线
11. &lt;big&gt;&lt;/big&gt; <big>放大字号</big>
12. &lt;small&gt;&lt;/small&gt; <small>缩小字号</small>
12. &lt;strong&gt;&lt;/strong&gt;   <strong>加粗</strong>
13. &lt;em&gt;&lt;/em&gt;  <em>斜体</em>


#### 页面布局
	
    先整体，后局部，先大后小顺序书写结构
    
#### 标签语义化
	标签要表示特定的意思

1. 有语义的标签
2. 无语义的标签
	1. 无默认的属性，主要用于设置样式
	1. div
	2. span

#### html 空元素
html空元素即为没有内容的html元素
这类元素在起始标签中进行关闭，没有特殊含义，可忽略
如 &lt;br /&gt;


#### html属性
1. html元素属性一般位于开始标签
2. 键值对形式出现
	1. 常用属性
		1. class
			1. 定义一个或多个类名（classname）
		2. id
			1. 定义唯一id
		3. style
			1. 设置内联样式
		4. title
			1. 描述元素的额外信息，作为工具条使用


#### 浮动

	可以让块元素排列一行
    
    float :left
    
    
#### 表单

![form.png](.\img\form.png)

![select.png](.\img\select.png)

1. name属性作为传值的key
2. value作为值
3. action表示数据提交目标
4. method提交方法
5. border-collapse:collapse,设置表格边线合并，因为单元格之间默认有间距
6. input
	1. placeholder  设定输入默认值
	
#### script


#### url

url只能使用ASCII字符集
数据传输前，会对其中的非ASCII编码字符进行转码，使用%后跟两位十六进制数标识
url中不能包含空格，通常使用+代替