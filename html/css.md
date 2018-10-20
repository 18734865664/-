### css
#### 概述
选择器 {属性：值；属性：值...}
内容和显示分离的思想

#### 引入方式
1. 内联样式：
	1. style=”“   属性
2. 嵌入式
	1. &lt;style&gt;代码块
	2. 写在head里面
	3. 一般在首页使用
	
3. 外链式
	1. 使用link引入
	2. 推荐使用


#### 选择器
1. 标签选择器
	1. 标签名
	2. 标签名1，标签名2...  可以同时定义多种类型
2. 类选择器
	1. .classId
	2. 名称自定义，不要与标签重名
	3. 一个标签，可以匹配多个类选择器
	4. 同一个页面可以出现多次

3. id选择器
	1. #myId
	2. 选择所有id="myId"的元素
	3. 同一个页面，只能出现一次
	4. 一个标签只能有一个ID
	5. 
3. 后代选择器
	1. 标签1 标签2 
	2. 中间空格隔开
	3. 选择所有标签1内中的标签2

5. 子选择器
	1. 标签a > 标签b
	2. 选择父元素为标签a的标签b
	3. 直系后代

6. 通用选择器
	1 *
    
7. 相邻同胞选择器
	1. 标签a + 标签b
	2. 紧跟在标签a后面的标签b

8. 属性选择器
	1. [attr]

9. 伪类选择器
	1. https://www.w3cplus.com/css3/pseudo-class-selector

10. 组选择器
	标签1， 标签2， 标签3... {}
    
11. 相邻兄弟选择器
	1. div ~ p
	2. div之后所有相邻兄弟元素

#### 选择器优先级
1. 不同级别
	1. !important > 内联 > ID选择器 > 伪类 = 属性选择器=类选择器 > 元素选择器[p]> 通用选择器(*) = 继承的样式

	2. style标识的选择器优先级，高于标签中单独定义
	3. ID选择器总是高于其他选择器的，计算优先级时
		1. !important = 1000
		2. id = 100
		3. class = 10 
		4. 标签选择器 = 1
		5. 用上面单位数学运算得出优先级数值，然后比较大小
		6. 但ID上级id永远比其他优先级要高， 所有不是简单的比较数值。
2. 同一级别
	1. 后写的覆盖先写的



#### 盒子模型

	将每个元素在页面中的形状，类比为一个盒子
    
![盒子模型.png](.\img\盒子模型.png)

1. 所有html元素都可以看作盒子（box method)
2. 每个盒子包括：
	1. Margin
		1. css中，margin可以是负值
	2. border
	3. padding
	4. content
		1. 设置的width/height 是指content区域的大小


1. 属性
2. 水平居中
	1. 
![水平居中.png](.\img\水平居中.png)

8. boader
	1. border-style
		1. dotted/dashed/solid/
	2. border-color
	3. border-top-style/border-right-style/border-bottom-style/border-left-style:solide;
	4. border-style: 指定边框类型（1, 2，3，4个参数，函数重载）
	5. border: border-width border-style border-color (简写)
	
9. outline
	1. 位于元素最外围（border外缘处）
		1. input输入框，默认有该属性
	2. outline: outline-style outline-width outline-color
	3. outline-style
	4. outline-color
	5. outline-width

10. margin
	1. margin 外边距，元素周围的空间
	2. 接受任何长度单位，百分数甚至负数，和auto
	3. margin完全透明，可以分别四个方向

11. padding
	1. 内边距 -- border 和content之间的空间
	2. 可以长度值和百分比，不能是负数
	3. 参数是百分比时，基数是父元素的长宽
	4. padding-top/padding-right/padding-bottom/padding-left
	5. padding : （1, 2，3，4个参数，函数重载）

3.背景
	
    background-color
    background-image
    background-repeat
    background-attachment
    background-position

![background.png](.\img\background.png)

4. Text
	1. color
		1. 文本颜色
	2. direction
		1. 文本方向
	3. letter-spacing
		1. 字符间距
	4. line-height
		1. 行高
	5. word-spacing
		1. 字间距
	5. text-align
		1. 水平对齐元素中文本内容
		2. text-align:center;
		3. text-align:left;
		4. text-align:right;
		5. text-align:justify;
			1. 实现两端对齐文本效果
		6. text-align:inherit;
	6. text-decoration
		1. 向文本添加修饰
		2. text-decoration:none;
			1. 一般用于去除下划线
		3. text-decoration:overline;
		4. text-decoration:line-through;
		5. text-decoration:underline;
	7. text-indent
		1. 首行缩进
		2. text-indent:50px;
	8. text-shadow
		1. text-shadow: h-shadow v-shadow blur color;
	9. text-transform
		1. 转换文本
		2. text-transform:uppercase;
		3. text-transform:lowercase;
		4. text-transform:capitalize;
	10. unicode-bidi
		1. 返回文本是否被重写
	11. vertical-align
		1. 设置垂直对齐
	12. white-space
		1. 元素中空白的处理方式
5. fonts
	1. font-family
		1. 文本字体
		2. font-family:"Times New Roman", Times, serif;
		3. 可以设置多种字体，如果前面的浏览器不支持，后移
	2. font-size
		1. font-size:40px;
			1. ie9之前版本不支持
		2. font-size:2.5em;
			1. em与当前字体大小相等
		3. font-size:100%;
			1. 浏览器默认文字大小16px
	3. font-weight
		1. 文字粗细
		2. normal|bold|bolder|lighter
6. link
	1. a:link -- 正常，未访问过的链接
	2. a:visited -- 用户已访问过的链接
	3. a:hover -- 用户鼠标放在链接上
	4. a:active -- 链接被点击的那一刻

7. list
	1. list-style-image: url('sqpurple.gif');background-repeat:no-repeat;
		1. 使用图片作为标签
	2. list-style-type
	3. list-style-position
	4. list-style-image
	5. list-style  按照上面三个顺序，简写属性

8. display/visibility
	1. 隐藏，显式元素
		1. display:none
			1. 隐藏元素，但不占空间
		2. display:inner
			1. 以内联元素形式显式
		3. display:block
			1. 按照块元素形式显式
		4. visibility:hidden
			1. 隐藏元素，占空间
	2. 内联元素与块元素转换
		1. 比如将a标签转换为块元素，则整片区域都可以进行跳转，且可以设置大小，而不需要包裹一层

9. overflow
	1. 元素溢出
	2. visible：默认值，内容不修改，在元素框外显示
	3. hidden：内容被修剪，其余内容不可见
	4. scroll：内容被修剪，显示滚动条（超没超出都会显示滚动条）
	5. auto：动态显示滚动条
	5. 在父级元素设置

10. dimension
	1. height
	2. line-height
	3. max-height
	4. min-height
	5. max-width
	6. min-width
	7. width

11. positioning
	1. 允许布局的一部分与另一部分重叠
	2. position:static
		1. html元素默认值，即不进行定位
	3. position:fixed
		1. 使用top, bottom, left, right属性进行定位
		<pre>
			position:fixed;
            top: 30px;
            right: px;</pre>
        2. fixed定位的元素，与当前文档流无关，因此不占空间
        3. 定位元素，相对于浏览器是固定的
    4. position:relative
		1. 相对其占用的空间，进行相对位移
		2. <pre>
			position:relative;
            top: 20px;
            left: 20px;
            z-index: -1;
        </pre>
    5. position:absolute
    	1. 绝对定位的元素位置相对于最近的已定位元素
    	2. 如果没有已定位的元素，则相对位置相对于&lt;html&gt;
    	3. absolute的定位与文档流无关，不占据空间
    6. 重叠的元素
    	1. z-index属性指定了堆叠顺序
    	2. 
    	<pre>
    	position:fixed;
            top: 30px;
            right: px;
            z-index: -1;
        </pre>
        3. z-index 可以是正数/负数


12. clip
	1. 裁剪一个已知img
	2. <pre>
	img{
   		position:absolute;
        clip:pst(0px, 60px,100px, 0px)
    }
	 </pre>
     
     
13. float
	1. 设定该属性，元素按指定的方向（left/right）向上漂浮，知道碰到块级元素或其他浮动元素
		<pre>
        	float: left;
            float: right;
        </pre>
	2. 元素浮动后，周围其他元素也会随之重拍
	3. 浮动元素之后的元素将围绕浮动元素
	4. 浮动元素之前的元素不受影响
	5. 清除浮动（clear)
		1. 元素指定方向，不能有浮动元素
		<pre>
        	clear: left|right|both|none|inherit
        </pre>
        
        2. 注意：clear只能控制元素本省，也就是说告诉元素本身在进行排版的时候，你左边，右边不能有浮动对象，其他元素的浮动的排布不受影响
    
14. 水平对齐
	1. 可以通过margin left/right设置auto实现
	2. padding
	3. position
 
15. 透明/不透明
	1. opacity:0.4
	2. opacity 取值范围是0.0-1.0之间，值越小，越透明
	3. 元素虚化
16. 图像拼合
	1. 图像拼合就是当个图像的集合
	2. 避免长时间，多次的向服务器请求图片（每次一个）
	3. 

17. box-sizing:
	1. 并排防止多个带边框的元素，好排版
	2. 
	<pre>
		 box-sizing: border-box;
         width: 30%;
         border: 1em solid red;
	</pre>
    
    3. box-sizing: border-box:
    	1. 指定的width，height， 确定元素的边框，也就是说包括了元素的padding和border，不只是content区域

18. 网格（grid）
	1. [grid学习文档](https://www.cnblogs.com/QianBoy/p/8626176.html)
	2. grid作为display的一个选项值；
	3. grid-template-rows
		1. 定义行数及行高，接受像素，百分比，fr(等分单位)
	4. grid-template-columns
	5. 
	<pre>
		 &lt;style&gt;
        .con {
            display: grid;
            /*repeat 一个简略函数*/
            grid-template-rows: repeat(3, 200px);
            /* grid-template-columns: 200px 200px 200px; */
            /* fr是等分单位，下面指分为三列，每列三等分 */
            grid-template-columns: 1fr 1fr 1fr;
            grid-gap: 2px; /*指定表格单元格之间间隔*/
            background-color: black
            
        }
        .con .box {
            background-color: azure;
            font-size: 40px;
            text-align: center;
            line-height: 200px;
        }
        .box:nth-child(2){
            /* grid-row-start: 2;      //指定起始网格管道
            grid-row-end: 3; 
            grid-column-start: 3;
            grid-column-end: 4; */
            grid-area: 2 /3 /3 /4;     /*上面内容的简写
            <row-start> / <column-start> / <row-end> / <column-end>;*/
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class="con"&gt;
        &lt;div class="box"&gt;1&lt;/div&gt;
        &lt;div class="box"&gt;2&lt;/div&gt;
        &lt;div class="box"&gt;3&lt;/div&gt;
        &lt;div class="box"&gt;4&lt;/div&gt;
        &lt;div class="box"&gt;5&lt;/div&gt;
        &lt;div class="box"&gt;6&lt;/div&gt;
        &lt;div class="box"&gt;7&lt;/div&gt;
        &lt;div class="box"&gt;8&lt;/div&gt;
        &lt;div class="box"&gt;9&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
	</pre>
    
    6. 	可以使用
