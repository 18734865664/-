### ES6

#### 简介
1. 在ES5增加了一些语法，所以完全兼容

#### 语法
##### 声明变量
1. let/const
2. 使用let/const声明变量，未声明则无法使用，没有默认值（var 变量有预解析，有默认值undefined）
		<pre>
        	alert(nNum1);   // 不报错，值未undefied
            var nNum1 = 11;
            
            alert(nNum2);   //报错
            let nNum2 = 22;
        	nNum2 = 44;     // 不报错
        
        	alert(nNum3);   // 报错
            const nNum3 = 33;
            nNum3 = 66;    //报错
        
        </pre>
3. let --> 变量， const --> 常量
4. 在新标准中，尽量使用const声明常量，避免因赋值改变引发错误，其次推荐使用let
5. 作用域不同
	1. var 是函数作用域
	2. let/const是块作用域（就是一对大括号）
	3. eg:
		<pre>
        function test(){
        	{
            	var n1 = 10;
                let n2 = 20;
                const n3 = 30;
            }
            console.log(n1);     //正常执行
            console.log(n2);   // 报错
            console.log(n3);    // 报错
        }
        
        </pre>

##### 数据类型
1. 分类
	1. 基本数据类型
		1. 字符串
		2. 数值
		3. 布尔型
		4. null
		5. undefined
	2. 复制类型
		1. 对象
		2. 数组
		3. 函数
1. 数组
	1. 数组是引用类型
		<pre>
        	let arr = [1,2,3]
            let arr2 = arr   // 此时两个变量指向了同一个底层数组，修改arr， 影响arr2
            
            // 创建新数组
            let arr3 = []
            for (i=0;i&lt;arr.length;i++){
            	arr3.push(arr[i]);
            };   // 这样获得一个独立的数组
            也可以：
            let arr4 = [...arr]
        </pre>
    2. 数组新增方法
    	1. map()
    		1. 遍历数组
    		2. <pre>
    			let arr = [1,2,3];
                arr.map(n => {
                	alert(n)
                })
            </pre>
    	2. concat()
    		1. 返回一个拼接后的数组，新数组与原数组无关。
    		2. <pre>
    			let arr = [1,2,3]
                let arr2 = [4,5]
                let arr3 = arr.concat(7,8)  // arr4 [1,2,3,7,8]
                let arr4 = arr.concat(arr2) // arr4 [1,2,3,4,5]
            </pre>
2. 字符串
	1. 格式化字符串
	2. 使用反斜线包裹字符串
	3. 变量用${}包裹


##### 解构赋值
1. 对象结构赋值
	1. 对象结构赋值时，被赋值变量必须跟对象中的定义一致
	2. eg:
	<pre>
    	let person = {name:"zhangsan", age = 11};
        // let {name, age} = person;  // 这里被赋值变量与对象定义语句中一致
        let {age , name} = person; // 顺序可以不一致
        console.log(`my name is ${name}, age is ${age}`);
        注意这里字符串格式化使用反引号包裹
    </pre>
    3. 解构赋值，可以选择性的赋值部分属性，而不是全部
    <pre>
    	let person = {name:"zhangsan", age = 11}
        let {name} = person
       	console.log(`${name}`)
    </pre>
2. 数组结构赋值
	2. 数组解构赋值，变量名可自定义
	<pre>
    	let arr = [1,2,3]
        let [a, b] = arr
        console.log(`${a}`)   // 这里
    </pre>
3. 扩展运算符（...）
	1. 用来作为数组的标识
	2. 赋值数组
		1. let arr3 = [...arr, newvalue1, newvalue2...]
	2. 用于向函数传递参数
		<pre>
        	function add(a,b,c){
            	return a + b + c
            };
            
            let arr = [1,2,3,4,5];
            add(...arr)  // 会将1，2，3依次传入函数
        </pre>
        
    3. 用于函数形参
    	<pre>
        	function add(...arr){
            	return arr
            }
            add(1,2,3,4,5)
            // 接受到的参数转变成了一个数组
        </pre>
        
##### 箭头函数
1. 箭头函数可以理解成匿名函数的第二种写法，箭头函数最主要的作用是可以在对象中绑定this关键字
	1. 匿名函数中的this指向window()对象，没绑定到匿名函数本身，使用箭头函数，可以将this绑定为箭头函数的父函数。
2. 匿名函数调用，需要将匿名函数包裹在小括号中，js特色
3. 不要再if/while代码块中定义函数，因为不同浏览器的解析结果不一致
2. 箭头函数普通写法
	1. <pre>
    	var Add = (a, b) => {
        	alert(a+b)
        }
        等价于
        var Add = function(a, b){
        	alert(a+b)
        }
    </pre>

3.	只有一个参数，可以省略小括号
	<pre>
    	var Echo = info => {
        	alert(info)
        }
    </pre>
    
4. 没有入参的箭头函数
	<pre>
    	var Hi = () => {
        	alert("hello")
        }
    </pre>    
    
4. 函数体中只有一个return语句,返回的是基本数据类型，可以省略大括号和return关键字
	<pre>
    	var Add = (a, b) => a+b
        等价于：
        var Add = function(a, b){
        	return a+b
        }
    </pre>
    
5. 函数体中返回的是一个对象，且只有一个return语句，对象要使用小括号括起来，大括号和return关键字可以省略
    <pre>
    	var Person = (name, age) => ({name:name, age: age})
        等价于
        var Person = function(name,age){
            return {name: name, age : age}
        }
    </pre>
    
6. 箭头函数可以绑定this关键字
	<pre>
		var Person = {
        	name: "tom";
            age: 11;
            showName : function(){
            	setTimeout(
                	() => {
                    	alert(this.name)
                    }
                )
                // 这里使用箭头函数绑定了this关键字指向本对象，如果使用普通匿名函数，由于进行了两层函数封装，this指向了window对象，造成错误的结果。
            }
        }
    </pre>

##### 对象的简写方式
<pre>
	es5对象写法：
    var Person = {
    	name:name,   
        age: age,
        showName: function(){
        	alert(this.name);
        },
        showAge: function(){
        	alert(this.age);
        },
    }
    
    
    简写方法：// 跟上面一一对应，简写方式
    var Person = {
    	name,    // 意思是作用域中如果存在与对象属性同名的变量，可以简写。
        age,
        showName(){
        	alert(this.name)
        },
        showAge(){
        	alert(this.age)
        },
    }
</pre>

##### 创建类和类的继承
1. es5 中没有class关键字，通过函数模拟对象
2. es6中增加了class关键字和constructor和super关键字的支持
3. 尽量使用class，避免使用prototype
4. 定义方法是，可以返回this关键字，用来实现链式调用
5. 定义一个toString()方法，但要确保不会引起其他副作用
3. 定义一个类：
	<pre>
    	class Person {
        	constructor(name, age){
            	this.name = name;
                this.age = age;
            }
            // 注意，es6中定义类函数，必须使用简写方式，不能使用ES5中的键值对形式
           	showName(){
            	alert(this.name);
            }

            showAge(){
            	alert(this.age);
            }
        }
        var andy = new Person("liudehua", 77);
        andy.showName();
        andy.showAge();
    </pre>

4. 继承类
	<pre>
    	class Student extends Person{
        	constructor(name, age, school){
            	super(name,age);
                this.school = shool;
            }
            // 重写方法
            showName(){
            	alert("this name is " + this.name)
            }
            showSchool(){
            	alert(this.school)
            }
        }
    </pre>
    
5. 对象实例化
	<pre>
    	// 需要new关键字
    	var obj1 = new Person("zhangsan", 222)
    </pre>
    
##### 异步请求数据
1. 使用promise对象封装ajax请求，封装后可以单独调用，也可以将多个promise对象合并调用（原子调用）；
2. promise封装后，promise.then(func)传入处理success状态的处理函数，catch(func)传入处理error状态的处理函数。
2. eg:
	<pre>
    	let prom1 = new Promise(function(resolve, reject){
        	$.ajax({
            	url: "xxxxx";
                type: "get";
                dataType: "json";
                success: function(data){
                	resolve(data);
                },
                error: function(data){
                	reject(data);
                }
            })
        })
        let prom2 = new Promise((resolve, reject) => {
        	$.ajax({
        		url: "xxxxx",
                type: "get",
                dataType: "json",
                success: data => resolve(data),
                error: data => reject(data)
        	})
        })
        // 单个Promise对象执行
        prom1.then(data => console.log(data)).catch(data => console.log(data));
        
        // 多个Promise对象原子执行
        Promise.all([prom1, prom2]).then(data => console.log(data)).catch(data => console.log(data));
    </pre>
    
    
    
    
##### 模块的导入导出
1. es6加入了模块的概念，一个js文件就是一个模块，js文件中需要先导出（export）后，才能被其他js文件导入（import）
2. 导入的变量名必须和导出的变量名一致
3. js文件中，从其他文件导入的变量，可以使用export导出去
2. 名字导出
	1. export let iNum01 = 12;
	2. export let fnMyfunc = data => {alert(data)}     // 创建同时声明导出
	3. export {iNum02, iNum2}  // 独立声明，集中导出
4. 名字导入
	1. import {iNum01, iNum02} from "./js/mode01.js";
	2. 需要指明script type
	3. &lt;script type="module">
3. 默认导出
	1. 默认导出名称和导入时名称可以不一样，可以用来导出匿名函数和类
	2. 每个js文件只能有一个默认导出
	3. export default {"name":"zhangsan", "age":33}
4. 默认导入
	1. import person from "./js/mode01.js"
	2. 需要指明script type
	3. &lt;script type="module">