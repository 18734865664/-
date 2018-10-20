### Reactjs

#### 前言
1. 使用react需要引用三个库：
	1. react.development.js   // react核心库
    2. react-dom.development.js  // 提供与dom相关的功能
    3. babel.min.js      // 可已经es6代码转换为es5代码，增强了兼容性
    4. 这三个引用的顺序不能变

2. react语法中不直接操作dom，已经进行了封装
3. 使用react JSX语法需要将script设置为 type = "text/babel"
4. ReactDOM.render(操作的元素，被操纵的dom对象,callback)
	1. eg
	<pre>
    	ReactDOM.render(
        	&lt;h1> helloworld&lt;h1>,
            document.getElementById("root")
        )
    </pre>
	2. 操作的元素只能是一个html元素，不能是多个，所以插入复杂结构时，使用div封装一下。
	3. 使用多个函数时，使用多对{}

5. 注释：{/* JSX中的注释 */}

#### JSX语法
1. 作用相当于是可以在JavaScript中直接写html代码，html和js语句写一块了
2. 是JavaScript的一种扩展语法

#### 组件
1. 返回一个JSX对象
2. 创建组件：
	1. 函数定义
	2. 类定义
	3. 组件名首字母需要大写
2. 通常通过类定义组件，可以使组件绑定属性和方法；
3. 