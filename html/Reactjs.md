# Reactjs

## 1. 前言

1. 使用react需要引用三个库：
  2. react.development.js   // react核心库
   3. react-dom.development.js  // 提供与dom相关的功能
   4. babel.min.js      // 可已经es6代码转换为es5代码，增强了兼容性
   5. 这三个引用的顺序不能变
6. react语法中不直接操作dom，已经进行了封装
7. 使用react JSX语法需要将script设置为 type = "text/babel"  // Babel是React团队选择的在使用React过程中转换ES*和JSX为ES5语句的工具，可以从[Babel handbook](https://link.jianshu.com/?t=https://github.com/thejameskyle/babel-handbook/blob/master/translations/en/user-handbook.md)了解Babel详细的用法。
8. ReactDOM.render(操作的元素，被操纵的dom对象,callback)
9. 操作的元素只能是一个html元素，不能是多个，所以插入复杂结构时，使用div封装一下。
10. 使用多个函数时，使用多对{}
11. 注释：{/* JSX中的注释 */}
## 2. 元素渲染

> 元素是构成React应用的最小单位，用于描述屏幕上输出的内容
>
> ​	const   ele = (
>
> ​		<div><h1>111</h1><h2>222</h2></div>  //
>
> ​	)
>
> 单个元素最高级元素只能是一个，不能是多个同级元素的集合，使用di包裹是一个好选择	
>
> 不同于DOM元素，React当中的元素事实上是普通的对象，ReactDOM可以确保浏览器DOM的数据内容与React元素保持一致
>
>  	1. React DOM 渲染的节点，称为“根”DOM节点
>  	2. 一般一个项目只定义一个根节点

### 2.1 更新元素

> React 元素都是不可变的。当元素被创建之后，你是无法改变其内容或属性的。
>
> 目前更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法：
>

## 3. JSX语法



1. 作用相当于是可以在JavaScript中直接写html代码，html和js语句写一块了
2. 是类型安全的（需要显式的转换类型）
3. 是JavaScript的一种扩展语法

#### 3.1 组件
1. 返回一个JSX对象
2. 创建组件：
  1. 函数定义

  2. 类定义

  3. 组件名首字母需要大写

     1. react通过首字母大小写，区分html标签和react本地组件

        ```react
        var myDivElem = <div className="foo"></div>;  // 只是一个html标签的变量，首字母小写
        var MyComponent = React.createClass({/*...*/});    // react本地组件就是一个react类/方法的对象，首字母必须大写
        var myElement = <MyComponent someProperty={true} />;
        ReactDOM.render(myElement, document.getElementById('example'));
        ```

3. 通常通过类定义组件，可以使组件绑定属性和方法；



### 3.2 文件引用

> react代码放到独立的文件中，使用script标签引入

```react
<script type="text/babel" src="helloworld_react.js"></script>
```

### 3.3 html/js混写

> JSX 中使用 JavaScript 表达式。表达式写在花括号 **{}** 

```
ReactDOM.render(
    <div>
      <h1>{1+1}</h1>
       <h1>{i == 1 ? 'True!' : 'False'}</h1>    // jsx中不能使用if...else语句，使用三元表达式
    </div>
    ,
    document.getElementById('example')
);
```

### 3.4 样式

> 使用内联样式

```
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
};
ReactDOM.render(
    <h1 style = {myStyle}>菜鸟教程</h1>,
    document.getElementById('example')
);
```

### 3.5 注释

> {/*注释...*/}

### 3.6 数组

> jsx中允许传入一个数组（html元素数组），jsx会将其自动展开

```react
var arr = [
  <h1>菜鸟教程</h1>,
  <h2>学的不仅是技术，更是梦想！</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

### 3. 7 符合组件

>  我们可以通过创建多个组件来合成一个组件，即把组件的不同功能点进行分离。

```jsx
		let arr = [
            <h1>hello</h1>,
            <h2>world</h2>
        ]

        function TestElem() {
            return <h3>helloworld</h3>
        }

        function MuxElem(props) {
            return (
                <div>
                    {arr}     // 多个元素组合起来，实现前端功能拆分
                    <TestElem/>
                    <span>{props.testtag}</span>
                </div>
            )
        }
```

## 4. React State

> React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
>
> React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。
>
> 通常在构造函数中完成this.state的初始化

```
			constructor(props){
                super(props)
                this.state = {
                    date : new Date()
                }
            }
```

> state 本身不能在嵌套组件之间传递，通过设定props的形式传递

```
	class Test1 extends React.Component{
            constructor(){
                this.state = {
                    name:"zhangsan"
                }
            }
            render(){
                return(
                    <Test2 name={this.state.name}/>   // 向内部元素传递数据
                )
            }
        }
        class Test2 extends React.Component{
            render(){
                return(
                        <h1>{this.props.name}</h1>
                )
            }
        }
```



## 5 this.props

> state 和 props 主要的区别在于 **props** 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。
>
> 构造函数，需要使用super(props)，显式的调用父类初始化函数
>
> 在class中使用this.props.name调用，在funcation中使用props.name调用

```
HelloMessage.defaultProps = {
  name: 'Runoob'
};
funcation Test(){
    return <h1>{props.name}</h1>
}
```



### 5. 1 设置默认值

> 组件都有一个默认的系统函数defaultProps

```
		class Clock extends React.Component{
            //TODO
        }

        Clock.defaultProps = {
            name: "test"
        }
		
		function TestFunc(props) {
            return <p>{props.name}</p>
        }

        TestFunc.defaultProps = {
            name: "test"
        }
```

### 5.2 props验证

> ```
> <script src="https://cdn.bootcss.com/prop-types/15.6.1/prop-types.js"></script>
> ```

## 6. 注意事项

> 1. 在添加属性时， class 属性需要写成 className ，for 属性需要写成 htmlFor ，这是因为 class 和 for 是 JavaScript 的保留字。