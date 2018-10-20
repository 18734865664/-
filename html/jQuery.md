{}### jquery

#### 概述
1. window.onload 和 $(document).ready()
	1. window.onload 等待html和数据都加载完之后执行方法
	2. $(document).ready(function(){...}) html加载后即执行，不需要等待图片等资源加载完成
	3. $(document).ready 可以简写成$(function(){...})
	4. 所有的jQuery都位于$(document).ready()中，防止文档没有加载完就执行
2. jQuery是为事件处理特别设计的
3. this关键字，jQuery中使用$(this)调用对象本身
4. jQuery是js的封装，所以js和jQuery和js是可以混用的
	1. <pre>
		
    </pre>

#### 语法格式

1. $(selector).action()
	1. $ 定义了使用jquery，可以理解为jquery封装的一个特殊函数
	2. selector  css选择器，用于定位html元素
	3. action 实际操作


#### 选择器
1. jQuery支持所有的css选择器对元素进行定位
2. jQuery选择出有多个结果时，组成一个结果集，（从0开始标识）
	1. 使用$("div").eq(3),eq方法从结果集中取对应对象
	2. 元素对象本身有一个索引值属性，通过index()方法获取索引值，这个索引值是指该元素在父级元素中同级元素的排位，不是在选择器结果集中的排位，需要进行区分
	<pre>
    	$("div").click(functin(){
        	$(this).index();
        })
    </pre>
2. 所有的选择器都以$作为标识： $(selector)
3. 表格：
	|语法 | 描述|
    |--|--|
    |$("*")| 选取所有元素|
    |$(this)| 选取当前HTML元素|
    |$("p.intro")|选取class 为intro 的&lt;p>元素|
    |$("p:first")|选取第一个&lt;p>标签|
    |$("ul li:first")|选取第一个&lt;ul> 元素的第一个&lt;li>元素|
    |$("ul li:first-child")|选择每个&lt;ul>元素的第一个&lt;li>元素|
    |$("[href]")|选取带有href属性的元素|
    |$("a[taget='_blank']")|选取所有target属性值等于"_blank"的&lt;a>元素|
    |$("a[taget!='_blank']")|选取所有target属性值不等于"_blank"的&lt;a>元素|
    |$(":button")|选择所有type="button" 的&lt;input>元素和&lt;button>元素|
    |$("tr:even")| 选取偶数位置的&lt;tr>元素|
    |$("tr:odd")| 选取奇数位置的&lt;tr>元素|

4. 选择器过滤
    1. $(selector).has("element")
    	1. 过滤出拥有"element"元素的元素集
    2. $(selector1).not(selector2)
    	1. 排除掉符合selector2的元素
    3. $(selector).eq(idx)
    	1. 从结果集中，提取索引为idx的元素
    	2. idx从0开始
    	3. 这里的idx是结果集中的索引
    	4. 元素本身有一个index()方法用于获取索引，这个索引是元素在父级元素中的位置索引，要做区分
    4. first()
    	1. 返回首个元素
    5. last()
    6. filter()
    	1. $(s1).filter(s2)
    	2. 从s1结果集中过滤出满足s2选择器的元素

5. 选择器转移
	1. $(selector).prev()
		1. 转移到上一个同级元素
	2. $(selector).prevAll()
		1. 转移到之前的同级元素集
	2. $(selector).prevUntil(selector2)
		1. 这种util方法，取的是两者之间的元素，调用者/参数这两个元素
	3. $(selector).next()
		1. 转移到下一个同级元素
	4. $(selector).nextAll()
		1. 转移到后面的同级元素集
		2. 可以过滤
		3. $(selector).nextAll(selector2)
	5. $(selector).nextUntil(selector2)
	5. $(selector).siblings()
		1. 转移到除自己外的所有同级元素集
		2. 可以接受参数过滤
	6. $(selector).parent()
		1. 转移到直接父级元素
	7. $(selector).parents()
		1. 获取所有的祖先元素，一直追溯到根元素（html)。
		2. $(selector).parents(selector2)
		3. 可以接受一个选择器，对结果集中内容进行过滤
	8. $(selector).parentsUntil(selector2)
		1. 返回selector和selector2之间的所有祖先元素
	8. $(selector).chidren()
		1. 转移到子元素集（只有直接子元素）
		2. $(selector).chidren(selector2)
		3. 查找selector下面所有的selector2元素集
	9. $(selector).find(selector2)
		1. 选择符合selector1元素集中符合selector2的元素集

6. 判断是否选择到了元素
	1. length属性
	2. $(selector).length == 0, 说明没有选择到元素；
	3. jquery中即使定位元素失败，也不会报错，只是返回一个空jquery对象；

#### 属性操作
1. 读样式属性
	1. var $sSize = $("#demo2").css("font-size");
	2. var $sSize = $("div").css("font-size");
		1. 当读取属性的对象是多个时，只返回第一个元素的结果

2. 写样式属性
	1. $("#demo2").css("color", "red");
		1. 只设置一个属性，不需要写成map形式
	2. $("#demo2").css("color": "red", "font-size":30)

#### 事件处理
1. 常见DOM事件
|鼠标事件|键盘事件|表单事件|文档/窗口事件|
|--|--|--|--|
|click|keypress|submit|load|
|dblclick(双击事件)|keydown|change|resize|
|mouseenter|keyup|focus|scroll|
|mouseleave||blur(失去焦点)|unload|
	1. submit事件阻止表单提交
		1. $(form).submit(function(){
        	return false;
        })
        
2. jquery响应事件设置
	1. <pre>
		$(selector).click(
        	function(){
            	...
            })
    </pre>

3. 原生js和jQuery中mouseenter
	1. 原生js中mouseenter和mouseleave 不支持冒泡模式
	2. jquery中则支持。
	3. hover会触发mouseenter + mouseleaver(完整动作)
	4. mouseenter 不支持子元素触发父元素的事件，就是说，鼠标位于子元素上，父元素的事件不会被触发，mouseover则会被子元素触发


#### 操作样式类名
1. addClass()    // 增加一个类名
	2. 注意参数不是选择器，而是类名
	3. 多个类名写到同一个字符串中
	4. $("#demo2").addClass("red big");
2. removeClass()    // 移除一个或多个类名
3. toggleClass()     // 对类名进行添加/删除操作切换   
	1. $("#box1").toggleClass("col2")

#### 事件冒泡和捕捉
##### 事件冒泡
1. 概念
	1. 事件触发后，由触发元素开始，逐级向上传递，直到传递到最外层的元素，最外层元素一般用$(document)表示
	2. 阻止事件冒泡：
		1. 使用event.stopPropagation();函数
		2. 简单写法: 绑定的函数 return false

##### 事件委托

1. 利用事件冒泡原理，把事件绑定到父级元素，通过判断时间来源的子集，执行相应的操作
2. 事件委托可以减少事件绑定的次数，提高性能
3. 可以让新加入的元素拥有相同的操作
4. code:
	<pre>
    	$(fatherSlector).delegate(sonSelector, event, function(){...})
        意思是在fatherSelector 绑定一个event事件，当子元素sonSelector触发event事件的时候，冒泡到父元素触发event事件，function(){...}中使用$(this)捕获触发事件的子元素对象
        $(".ul").delegate("li", 'click', 
        function(){
        	$(this).css("background":"red")
        })
    </pre>


#### 效果
1. 隐藏和显示
	1. hide(speed, callback)
		1. speed, callback都是可选项
		2. speed是速度，单位是毫秒,或者slow, "fast"
		3. callback是执行完隐藏操作后，执行的函数名称；
		4. <pre>
		$("#demo2").hide(1000, function(){
        	console.log("hiding...")
        });
		 </pre>
	2. show(speed, callback)
	3. toggle(speed, callback)
		1. 自行在隐藏和显示之间进行切换
		2. callback函数，jQuery选择结果集中有几个元素，callback哈数就会执行几次，
		3. 如果callback后面有()，则callback会立即执行，而不是等待效果结束
	4. 例子：
		<pre>
$(this).removeClass("now other").addClass("now").siblings().addClass("other");
        
        $(".info").eq($(this).index()).removeClass("Show Hid").addClass("Show").siblings().addClass("Hid");
        </pre>

2. 动画
	1. animate()
	2. $(slector).animate({params}, speed, model, callback);
		1. params: 动画效果的css样式键值对。(必须设置)
			1. k:v,k:v...
			2. 中括号是必须的
		2. speed
		3. model: 
			1. swing 缓出 // 默认值
		    2. linear 匀速
		4. callback;
	3. 注意：animate()可以操作几乎所有的css样式，必须使用camel标记法书写属性名，如：paddingLeft 而不是padding-left
	4. 原生jquery不支持制作彩色动画，需要下载Color Animations插件
	5. params中赋值可以使用相对值：
		1. "left" : "+=150px"

	6. params中属性值可以是jQuery中预定义值
		1. show, hide, toggle
		2. eg: height : "toggle";

3. 淡入/淡出
	1. $(selector).fadeIn(speed,callback)
	2. $(selector).fadeOut(speed,callback);
	3. $(selector).fadeToggle(speed,callback);
	4. $(selector).fadeTo(speed,opacity,callback);
		1. fadeTo() 允许渐变为给定的不透明度（介于0~1之间）

4. 滑动
	1. $(selector).slideDown(speed,callback);
		1. 与show是由差别的，show函数由定位点，横向纵向同时扩展，至设置的大小
		2. slideDown只是纵向线下扩展。
	2. $(selector).slideUp(speed,callback);
	3. $(selector).slideToggle(speed,callback);

5. jQuery对象队列
	1. jquery对象快速多次触发事件后，会在系统中保留一个队列，依次执行，如：多次触发slideToggle，即使鼠标已经移走，仍然会多次下滚，卷起
	2. 清除队列stop()方法；
	3. $(selector).stop().slideToggle(speed, callback).
	4. stop()方法会将前面的事件队列清空，执行最后一个事件响应
	5. $(selector).stop(stopAll,goToEnd);
		1. stopAll: 是否应该清除动画队列，默认是false，即仅停止活动的动画，允许后面的动画执行
		2. goToEnd: 规定是否立即完成当前动画

6. 链式调用
	1. jQuery中每次调用函数后，默认会返回其选择结果集对象，后面仍然可以继续调用方法：
	2. $(this).removeClass("now other").addClass("now").siblings().addClass("other");

7. callback
	1. jQuery中不同元素对象事件执行可以看作是异步操作，造成前面的特效还没结束，后面的效果开始执行
		1. <pre>
		$("button").click(function(){
  			$("p").hide(1000);
  			alert("The paragraph is now hidden");    // alert会在卷起之前弹出。这不合理
		});
        </pre>
	2. 遇到有执行顺序要求的时候，添加到callback中，可以解决这种问题

#### jQuery HTML

1. 获取内容
	1. text()
		1. 设置或返回所选元素的文本内容
		2. 如果selector是一个结果集，会获取全部元素及其子元素的文本内容
	2. html()
		1. 设置或返回所选元素的文本内容（包括HTML标记）
		2. selector是一个结果集，会获取结果集中第一个元素包含的内容
	3. val()
		1. 设置或返回表单字段的值（value属性）
	4. 调用时空括号是读取，括号包含了内容，是覆盖原内容
	5. 回调函数
		1. text()，html()设定值的时候可以使用回调函数
		2. test(function(index, olderStr)),
		3. html(function(index, olderStr))
		4. 这个回调函数传入两个参数，一个是当前元素在结果集中的索引值，第二个是原来的文本内容。
		5. 回调函数作用是加工数据后，返回一个新的文本内容，用新文本内容赋值元素
		6. 例子
		$(".box1").html(function(index, older){
                return "hello" + older + index；});
        7. 类似python中的装饰器
2. 获取属性
	1. attr("paramter")
		1. 设置单个属性
				$("#w3s").attr("href","//www.w3cschool.cn/jquery");
		2. 设置多个属性
		<pre>
		$("#w3s").attr({
    		"href" : "//www.w3cschool.cn/jquery",
    		"title" : "jQuery 教程"
  		});
        </pre>

 		3. 设置多个属性，以键值对的形式传入
		4. 使用回调函数
			1. $(selector).attr(attribute,function(index,oldvalue))
	2. prop()
		1. 用法与attr相同
	3. prop()/ attr() 之间的区别
		1. attr() 比prop()要老，
		2. attr()设置属性的时候，有时候会有结果不一致的情况
		3. 具有 true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()，其他的使用 attr()，具体见下表：
		4. 
![attr_prop.png](.\img\attr_prop.png)

3. 添加元素
	1. $(selector).append(selector2)   // 元素内
		1. 被选selector元素内部结尾追加插入指定内容selector2
		2. 也可以添加新创键标签 将selector2换成$("&lt;div>value&lt;/div>")
	2. prepend()  // 元素内
		1. 被选元素内部开头插入指定内容
	3. after()   // 元素外
		1. 被选元素后面插入内容
	4. before()  // 元素外
		1. 被选元素前面插入内容
	5. eg:
		$("#box").append("world").prepend("说在前面\n").after($("&lt;div style='color: red;'>插入div&lt;/div>")).before($("&lt;div style='color:blue'>插在前面div&lt//div>"));
    6. $("&lt;div>"),尖括号括起来，标识创建了一个空的div标签
       
4. 删除元素
	1. remove()
		1. 将选定的元素及其子元素删除
		2. $(selector1).remove(selector2)
			1. remove() 接受一个选择器作为过滤参数，可以有选择的删除元素
	2. empty()
		1. 将选定的元素清空，删除子元素，文本内容

4. 复制元素
	1. $(selector).clone(true/false)
	2. 复制选中的元素
	3. 参数为true,则复制元素及绑定的事件
	4. 参数为false，则复制元素，不复制事件

5. 操纵样式
	1. 获取属性
		1. $(selector).css("background-color")
	2. 设置属性
		1. $("p").css("background-color","yellow");

6. 尺寸
	1.  | jQuery尺寸 | css尺寸 |
		|--|--|
		|width()|width|
        |innerWidth()|width + padding * 2|
        |outerWidth()|width + padding * 2 + border * 2|
        |outerWidth(true)|width + padding * 2 + border * 2 + margin * 2|
        |height()|height|
        |innerHeight()|height + padding * 2|
        |outerHeight()|height + padding * 2 + border * 2|
        |outerHeight(true)|height + padding * 2 + border * 2 + margin * 2|
	2. 使用方式
		1. $(selector).width()

#### ajax
1. load()
	1. 用于从服务器加载数据，并把放回的数据放入被选元素中
	2. $(selector).load(url, callback(resultText,statusText, xhrObj))
	3. <pre>
		$("button").click(() => {
                $("#box").load("http://127.0.0.1:8080/data", (data) => console.log(data));
            })
    
    </pre>
2. get()
	1. get方法可能返回缓存数据
	2. $.get(url, data, function, dataType)
3. post()
	1. post方法不会缓存数据
4. $.ajax()
	1. 常用参数：
		1. url
		2. type: 'get'/'post'
		3. dataType: 数据返回的格式 'json'/'text'
		4. data： 发送给服务器的数据
		5. success：设置请求成功后的回调函数
		6. error：设置请求失败后的回调函数
		7. async：设置是否异步，默认“true",表示异步
		
    2. $.ajax({
    	url: "http://...",
        type: "get",
        dataType: "json",
    	...
    })
    3. 回调函数可以简写
    	<pre>
    	$.ajax({
            ....
        }).done(function(data){...}).fail(function(data))
        //done 表示成功响应
        //fail 表示失败响应
        </pre>