### javascript

#### 简介
1. 解释型语言，不很严谨
2. html中的脚本，必须位于&lt;script&gt;&lt;/script&gt;之间
3. 可以位于head/body/单独文件中
4. 引用方式
	1. 行间时间（直接在标签中定义）
	2. 页面script标签嵌入
	3. 外部引入
		1. &lt;script src="myScript.js"&gt;&lt;/script&gt;
5. 弱类型语言
6. 逐行执行代码
7. javascript语句是发给浏览器的命令
8. js和html最后都会进行代码压缩（去除空格，制表符）
9. javascript 全局变量，在页面关闭时才会释放，意味着，页面打开的整个生命过程中，javascript都在运行，中间的函数调用中止只是函数结束了，而不是javascript结束
10. js预解析特性
	1. 函数调用和定义顺序不需要强制
11. 有this关键字（操作自身特性使用）
	1. onclick="this.innerHTML=Date()"
12. js中所有事物都是对象

#### 常用函数
1. 直接写入HTML输出流
	1. document.write("&lt;p&gt;this is  a title&lt;/p&gt;")
	2. 只能在html输出中使用，如果在文档加载结束使用该方法，会覆盖整个文档
	
2. 弹出警告（alter)
	1. alter("hello")
3. 改变html内容（innerHTML)
	1. 元素里面包裹的内容
4. 获取dom对象
	1. x = document.getElementById("demo")
	2. x = document.getElementsByClassName("demo")
	3. document.getElementsName("name")
		1. 注意，由于name在html文档中并不唯一，所以取回的是一个对象的集合，而不是一个具体的对象，所以需要遍历该集合进行取值赋值
		2. 不能直接通过索引操作
	4. 获取的dom对象，可以赋值给一个变量

5. 改变Html样式
	1. 先获取对应的dom对象
	2. ![改变html元素样式.png](.\img\改变html元素样式.png)
	3. 注意：替换元素的class属性，需要写className，才能识别到属性。
6. 验证输入是否数字（isNaN(arg)）
7. window.onload  // 等页面加载完，才执行代码
    1. <pre>
	 window.onload = function(){
            alert("加载完成")
        }
    	 </pre>

8. Math.random()
	1. 返回0-1之间的随机数

	 
#### javascript 输出
1. window.alter()
2. document.write()
	1. 写入文本和HTML。
3. innerHTML
4. console.log
	1. 写入到浏览器控制台


#### 常见事件（html DOM事件）
1. onchange  html元素改变
2. onclick   点击事件
3. onmouseover 获得鼠标焦点
4. onmouseout  鼠标移开
5. onkeydown   按下了键盘按键
6. onload      浏览器完成页面加载
7. onfocus     获取焦点


#### 定时器
1. setTimeout
	1. 只执行一次的定时器
	2. var oTick = setTimeout(myFunc, 2000)
	3. 默认单位毫秒
2. clearTimeout
	1. clearTimerout(oTick)
3. setInterval
	1. 周期执行的定时器
	2. var oTimer = setInterval(myFunc, 2000)
	3. 
4. clearInterval


#### 表单验证
1. 获取表单元素
	1. var oF = document.forms["formName"]["elName"].value
2. 为表单绑定校验函数
	&lt;form onsubmit="myFunc()"&gt;
    
#### json
1. 语法规则
	1. 数据为键值对
	2. 逗号分隔
	3. 大括号保存对象
	4. 方括号保存数组
	5. json中不支持函数元素和undefined
2. json与字符串转换
	1. json --> string
		1. var sTr = obj.toJSONString();
	2. String --> json
		1. var obj = JSON.parse(text);
		2. 转换成字符串后，就可以使用点号操作元素
	3. json中字符串必须使用双引号

#### javascript: void
1. void 是一个操作符，表示执行一个表达式，但是不返回值
	1. a = void( b = 5, c = 6, b+c)
	2. 这个表达式，为b,c赋值，执行b+c，但是没有返回值，不能给a赋值
2. javascript:
	1. 这是一个伪协议，表示 &lt;a&gt; 标签中url不进行跳转，而是由一个javascript函数执行
	2. &lt;a url="javascript:funcName()"&gt; 在点击链接的时候，调用funcName() 函数；
3. javascript:void(0)
	1. 这表示执行了一个空函数，点击超链接，不执行任何操作
	2. &lt;a&gt url="#";  点击后会跳至一个锚点，默认锚点在页首



#### dom树
1. 概述
	1. 当网页被加载时，浏览器会创建页面的文档对象模型（document object model)
	2. 通过html DOM可以访问所有的html元素及其文本和属性
	3. html DOM独立于语言和平台，可以被任何变成语言使用
	4. JavaScript通过dom可以操作页面中所有html元素及其属性，样式，可以对所有事件作为反应。
2. 查找html元素
	1. 三种方法
	2. 通过id查找
		1. document.getElementById("intro");
		2. 返回该元素的对象
	3. 通过标签签名查找
		1. var x = document.getElementById("main");
		2. var y = x.getElementByTagName("p");
		3. 查找id = main的元素中所有的p标签
		4. 返回一个对象列表
	4. 通过类名查找
3. 改变html内容
	1. 改变输出流
		1. document.write();
			1. 直接向html输出流些内容
			2. 在网页加载完成后，绝对不可以使用document.write，会覆盖原网页
		2. documentObj.innerHtml = new HTML;
			1. innerHtml属性，可以获取并操作html元素包裹的内容；
4. 改变html属性
	1. document.getElementById(id).attribute = new value
5. 改变样式
	1. document.getElementById(id).style.attribute = new value;
6. 对事件进行响应
	1. 点击事件
		1. document.getElementById(id).onclick = function(){...}
			1. onclick 是一次完整的点击
		2. onmousedown
		3. onmouseup;
	2. 鼠标移入
		1. document.getElementByid(id).onmouseover = function(){...}
		2. onmouseout，与onmouseover配套使用;
		3. onmouseenter; 效果与onmouseover类似，区别在于onmouseenter不支持冒泡，只有外层元素的事件监听会被触发
		4. onmouseleave，与onmouseenter配套使用
		5. 
	3. 网页已加载时
	4. 图片已加载时
	5. 输入字段被改变时
		1. onchange
	6. 提交html
	7. 用户触发按键
	8. onload 和onunload事件
		1. onload在用户进入页面时触发，可用于检测浏览器类型/版本，加载正确的版本
		2. onunload 可用于处理cookie
	8. 这里有个技巧，可以使用this关键字将对象本身传入函数进行处理
		1. <pre>
			&lt;h1 onclick="changetext(this)"&gt;haha&lt;h1&gt;
        
        &lt;script> 
        function changetext(id){
        	id.innerHTML = "hehe"
        }
        &lt;/script>
		</pre>
7. DOM EventListener
	1. 向指定元素添加事件句柄（addEventListener())
		1. 使用addEventListener添加的事件监听，不会覆盖已存在的事件句柄。
		2. 一个元素可以同时存在多个事件函数（同一监听类型也可以存在调用多个函数的句柄，如两个click事件）
		3. 可以向任何DOM对象添加事件监听，不仅仅时HTML元素。如：window对象
		4. 语法：element.addEventListener(even, function, useCapture)
			1. even: 事件类型，如click，mousedown等，注意没有on前缀
			2. function：监听到的事件触发的函数
			3. 控制事件执行顺序：
				1. 冒泡（false 默认值）：嵌套的元素激发事件监听时，内部元素对象的事件监听先触发，再触发外部元素的监听事件
				2. 捕获（true): 先执行外部元素的事件监听
		5. eg：
			1. document.getElementById(id).addEvenListener("click", function(){...}, false)
			2. window.addEvenListener("resize", function(){...})
				1. resize是window对象的事件，表示页面大小发生变化。
				
	2. 移除事件监听
		1. element.removeEventListener(even, function)
		2. 这里function需要与addEventlistener中function。

8. DOM 元素
	1. 在DOM中，每个节点都是一个对象，有三个重要的属性，分别是：
		1. nodeName: 节点名称
		2. nodeValue: 节点的值
		3. nodeType: 节点的类型
	2. 创建新的html元素
		1. <pre>
			&lt;div id = "div1">
            &lt;p id = "p1"> 11111111111&lt;/p>
            &lt;p id = "p2"> 2222222&lt;/p>
        	&lt;/div>
            &lt;script>
            var para = document.createElement("p");
            var node = document.createTextNode("3333333");
            para.appendChild(node);
            
            var element = document.getElementById("div1");
            element.appendChild(para);
            &lt;script>
        </pre>
	3. 删除已有的HTML元素
		1. <pre>
			&lt;div id = "div1">
            &lt;p id = "p1"> 11111111111&lt;/p>
            &lt;p id = "p2"> 2222222&lt;/p>
        	&lt;/div>
            &lt;script>
            var parent = document.getElementById("div1");
            var child = document.getElementById("p1");
            parent.removeChild(child);
            /*
            	js中删除dom元素必须知道父级元素，可以取巧：
                child.parentNode.removeChild(child);
            */
        </pre>


#### javascript 对象

	对象只是一种特殊的数据，对象拥有属性和方法
    es5中JavaScript面向对象最大的区别，没有class关键字，使用函数模拟类生成对象
    
1. 创建对象
	1. 创建无类型对象
		1. 先声明，后赋值
			<pre>
        	var oPer = new Object();
            oPer.name = "zhangsan";
            oPer.age = 11;
        	</pre>
        2. 声明同时赋值
        	<pre>
           	var oPer = {name :"lisi", age: 22};
            </pre>
	2. 创建类型对象(使用构造函数)
		1. 先声明，后赋值
			<pre>
            function Person(name, age){
            	this.name = name;
                this.age = age;
            }
            var oPer = Person;
            oPer.name = "zhuliu";
            oPer.age = 44;
            </pre>
        2. 声明同时赋值
        	<pre>
            var oPer = new Person("wangwu", 33);
            </pre>
            
2. 对象属性
	1. js中对象的属性并不是固定的，可以随时向对象中添加新的属性（使用赋值运算）
		<pre>
        	var oPer = {name : "zhangsan"};
            oPer.age = 33;
            // 此时对象已经具有了age属性，简单粗暴！
        </pre>
	2. 使用for(i in obj){} 可以遍历对象的属性；
		<pre>
        	ver oPer = {name : "zhangsan", age : 33};
            for (s in oPer){
            	alert("属性" + s + ": " + oPer[s]);
            }
        </pre>
	3. 

3. 对象的方法
	1. js中对象的方法，其实就是构造器函数内的一项属性值，属性对应的值是一个函数；

1. 数字对象
	1. js中只有一种数字类型，通过Number()构造函数创建。
	2. 所有数字对象均为64位
	3. 整数位数最多为15位， 浮点运算可能丢失精度；
	4. 无穷大（Infinity)和负无穷大（-Infinity)
		1. <pre>
			var iNum = 33;
            if (iNum != Infinity){
            	alert("seccuss");
            }
		</pre>
    5. 非数值NaN(指某个数不是数字)
    	1. isNaN(x)，判断x不是数字

2. 字符串对象
	1. 几乎所有对象都可以通过toString()转换位字符串（js中有隐式的类型转换）
	2. length属性
	3. 可以使用索引（0开始）

3. 时间对象
4. 数组对象
5. Boolean对象
	1. 跟python类似，Boolean对象的默认值是false（一切类型的默认值，都可以认为是false，0, -0,null等）
6. Math对象
	1. 用于常见的算术任务；
	2. 常用方法：
		1. random()
		2. round()   // 用于四舍五入
		3. max()
		4. min()
		5. PI()
7. RegExp对象
	1. 格式：
		1. patt = /world/i
			1. 两种模式：i(大小写不敏感)，g(全文查找)
			2. world（表达式不需要括号）
	2. 方法：
		1. String.match(patt)
		2. patt.test(String) bool // 判断是否拥有
		3. patt.exec(String) String// 返回匹配字段

8. window对象
	1. window对象标识浏览器中打开的窗口或一个框架，在客户端JavaScript中，window对象是全局对象，所有表达式都在当前环境中计算；
	2. window子对象
		1. JavaScript document对象
		2. JavaScript frames对象
		3. JavaScript history 对象
		4. JavaScript location对象
		5. JavaScript navigator对象
		6. JavaScript screen对象
		7. 
![window对象.gif](.\img\window对象.gif)

9. JavaScript execCommand函数
	1. document.execCommand(sCommand[,交互方式, 动态参数]) 其中：
		1. sCommand为指令参数（如下例中的”2D-Position”）
		2. 交互方式参数如果是true的话将显示对话框，如果为false的话，则不显示对话框（下例中的”false”即表示不显示对话框），
		3. 动态参数一般为一可用值或属性值（如下例中的”true”）。
	2. 很好用，比如：
		<pre>
       &lt;div class="demo">
        	helloworld
        	&lt;button id = "demo1">点击&lt;/button>
    &lt;/div>
    &lt;script>
        	document.getElementById("demo1").addEventListener("click", function(){
            document.execCommand("copy");
        })
    &lt;/script>
        
        </pre>
	3. 基础指令参数及意义：
	2D-Position 允许通过拖曳移动绝对定位的对象。
AbsolutePosition 设定元素的 position 属性为“absolute”(绝对)。
BackColor 设置或获取当前选中区的背景颜色。
BlockDirLTR 目前尚未支持。
BlockDirRTL 目前尚未支持。
Bold 切换当前选中区的粗体显示与否。
BrowseMode 目前尚未支持。
Copy 将当前选中区复制到剪贴板。
CreateBookmark 创建一个书签锚或获取当前选中区或插入点的书签锚的名称。
CreateLink 在当前选中区上插入超级链接，或显示一个对话框允许用户指定要为当前选中区插入的超级链接的 URL。
Cut 将当前选中区复制到剪贴板并删除之。
Delete 删除当前选中区。
DirLTR 目前尚未支持。
DirRTL 目前尚未支持。
EditMode 目前尚未支持。
FontName 设置或获取当前选中区的字体。
FontSize 设置或获取当前选中区的字体大小。
ForeColor 设置或获取当前选中区的前景(文本)颜色。
FormatBlock 设置当前块格式化标签。
Indent 增加选中文本的缩进。
InlineDirLTR 目前尚未支持。
InlineDirRTL 目前尚未支持。
InsertButton 用按钮控件覆盖当前选中区。
InsertFieldset 用方框覆盖当前选中区。
InsertHorizontalRule 用水平线覆盖当前选中区。
InsertIFrame 用内嵌框架覆盖当前选中区。
InsertImage 用图像覆盖当前选中区。
InsertInputButton 用按钮控件覆盖当前选中区。
InsertInputCheckbox 用复选框控件覆盖当前选中区。
InsertInputFileUpload 用文件上载控件覆盖当前选中区。
InsertInputHidden 插入隐藏控件覆盖当前选中区。
InsertInputImage 用图像控件覆盖当前选中区。
InsertInputPassword 用密码控件覆盖当前选中区。
InsertInputRadio 用单选钮控件覆盖当前选中区。
InsertInputReset 用重置控件覆盖当前选中区。
InsertInputSubmit 用提交控件覆盖当前选中区。
InsertInputText 用文本控件覆盖当前选中区。
InsertMarquee 用空字幕覆盖当前选中区。
InsertOrderedList 切换当前选中区是编号列表还是常规格式化块。
InsertParagraph 用换行覆盖当前选中区。
InsertSelectDropdown 用下拉框控件覆盖当前选中区。
InsertSelectListbox 用列表框控件覆盖当前选中区。
InsertTextArea 用多行文本输入控件覆盖当前选中区。
InsertUnorderedList 切换当前选中区是项目符号列表还是常规格式化块。
Italic 切换当前选中区斜体显示与否。
JustifyCenter 将当前选中区在所在格式化块置中。
JustifyFull 目前尚未支持。
JustifyLeft 将当前选中区所在格式化块左对齐。
JustifyNone 目前尚未支持。
JustifyRight 将当前选中区所在格式化块右对齐。
LiveResize 迫使 MSHTML 编辑器在缩放或移动过程中持续更新元素外观，而不是只在移动或缩放完成后更新。
MultipleSelection 允许当用户按住 Shift 或 Ctrl 键时一次选中多于一个站点可选元素。
Open 目前尚未支持。
Outdent 减少选中区所在格式化块的缩进。
OverWrite 切换文本状态的插入和覆盖。
Paste 用剪贴板内容覆盖当前选中区。
PlayImage 目前尚未支持。
Print 打开打印对话框以便用户可以打印当前页。
Redo 目前尚未支持。
Refresh 刷新当前文档。
RemoveFormat 从当前选中区中删除格式化标签。
RemoveParaFormat 目前尚未支持。
SaveAs 将当前 Web 页面保存为文件。
SelectAll 选中整个文档。
SizeToControl 目前尚未支持。
SizeToControlHeight 目前尚未支持。
SizeToControlWidth 目前尚未支持。
Stop 目前尚未支持。
StopImage 目前尚未支持。
StrikeThrough 目前尚未支持。
Subscript 目前尚未支持。
Superscript 目前尚未支持。
UnBookmark 从当前选中区中删除全部书签。
Underline 切换当前选中区的下划线显示与否。
Undo 目前尚未支持。
Unlink 从当前选中区中删除全部超级链接。
Unselect 清除当前选中区的选中状态。


#### BOM（Browser Object Model）对象
1. Window对象
	1. 是BOM所有对象的核心，所有对象的父对象
	2. 包含一些窗口控制函数
	3. 所有浏览器都支持window对象，它表示浏览器窗口
	4. 所有javascript全局对象/函数/变量均自动成为window对象的成员
	5. HTML DOM的document也是window对象的属性之一
		1. window.document.getElementById(id)
		2. 等价于 document.getElementById(id)
	6. window尺寸（不包括工具栏，滚动条）
		1. window.innerWidth(大多数浏览器)
		2. window.innerHeight
	7. 其他方法
		1. window.open()  //打开新窗口
			1. 三个参数：
				1. url
				2. name
				3. style
		2. window.close()  // 关闭当前窗口
		3. window.moveTo()   // 移动当前窗口
		4. window.resizeTo()  // 调整窗口尺寸

2. Screen对象
	1. window.screen 对象包含有关用户屏幕的信息
	2. 跟document一样，screen调用属性时，可以省去window
	3. 可用的长宽
		1. console.log("width: ", screen.availWidth);
		2. console.log("height: " + screen.availHeight)
		3. 都去掉了菜单栏值类的系统自留地

3. location对象
	1. 用于获得当前页面的地址（url）， 并把浏览器重定向至新页面
	2. 属性：
		1. location.hostname
		2. location.pathname
		3. location.port
		4. location.protocol
		5. location.href
	3. 方法：
		1. assign()
			1. 在当前页面加载新文档
			2. location.assign("http://www.baid.com")
4. history对象
	1. 包含浏览器的历史
	2. 方法：
		1. history.back()
			1. 相当于在浏览器中点击后退按钮
		2. history.forward()
			1. 相当于在浏览器中点击向前按钮

5. Navigator对象
	1. 包含有关访问者浏览器的信息
	2. 属性：
		1. navigator.appCodeName  // 浏览器类型
		2. navigator.appName    // 浏览器名称
		3. navigator.appVersion    // 浏览器版本
		4. navigator.cookieEnabled   // 是否启用cookies
		5. navigator.platform    // 硬件平台
		6. navigator.userAgent    // 用户代理
		7. navigator.systemLanguage   // 用户代理语言
	3. navigator中的信息可被浏览器使用者更改，所以不能用于客户端信息的采集，会被误导。
	4. 旧版浏览器无法反馈新版本的操作系统信息
	5. 可以利用不同浏览器兼容性判断浏览器类型（比如只有opera支持window.opera，可以作为判别依据

6. 弹窗
	1. 警告框
		1. window.alert(str)
		2. 会阻塞程序
	2. 确认框
		1. window.confirm(str)
		2. 需要选择确认还是取消
		3. 选择确认则返回true, 取消则返回false

	3. 提示框
		1. window.prompt(tip, defaultValue)
		2. 弹出后需要输入一些信息
		3. 选择确认或取消
		4. 选择确认，返回填入的内容
		5. 选择取消，返回null

	4. 换行符
		1. "\n"
7. 定时器
	1. 单次定时器
		1. setTimeout(function, milliseconds)
		2. clearTimeout(定时器变量)
	2. 周期定时器
		1. setInterval(function, milliseconds)
		2. clearInterval(定时器变量)
	3. 不需要写单位，毫秒值
8. Cookies
	1. 用于存储web页面的用户信息
	2. 
