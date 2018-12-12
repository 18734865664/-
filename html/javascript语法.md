### javascript 语法
#### 变量
1. 关键字 var
2. 类型
     1. string
     2. number
     3. boolean
     4. object
      5. Object
      6. Date
      7. Array
     8. function
     9. 空数据类型
           1. null
           2. undefined
     10. 数字
      11. 整数
      12. 小数
   3. 科学计数
   4. 运算符：

        		1. ++， -- 与go不同，是表达式，不是语句
  5. 字符串
   6. 单双引号都可以
   7. 两种方式创建
        		1. var sTestA = "hello"
          		2. var sTestB = new String("world")
   8. 属性
        		1. length
          		3. prototype
   9. 方法
            		1. charAt()
                       		1. 返回指定索引位置的字符
            		2. concat()
                       		1. 用于链接字符串
            		3. indexOf(), lastIndexOf()
            		4. localCompare()
                       		1. 比较字符串
            		5. match()
            		6. replace()
            		7. search()
            		8. slice()
                       		1. 提取字符串片段
            		9. split()
            		10. substring()
                        		1. 提取两个指定索引之间的字符
            		11. toLowerCase()
            		12. toUpperCase()
            		13. toString()
            		14. trim()
                        		1. 去空格
            		15. valueOf()
   10. 字符串与数字相加，自动转换为字符串
  11. 表达式
  12. 数组（array）
   13. [1, 2, 3, 4, 5]
   14. 定义
         		1. var aList01 = new Array(1,2,3);
           		2. var aList02 = [1,2,3,4,5];
   15. 属性
         		1. length
           		2. 
   16. 方法
             		1. push()
                        		1. n = aList01.push(4)
                        		2. 返回值n为数组长度
             		2. pop()
                        		1. obj = aList01.pop()
                        		2. 返回弹出的元素
             		3. reverse()
                        		1. var aLIst02.reverse()
                        		2. 这里很奇怪，reverse()返回反转后的数组，同时将原数组也进行了反转，既有返回值，有操作了原数组
             		4. var iPos = aList01.indexOf(1)
                        		1. 如果数组中有1， 则返回第一个匹配的索引
                        		2. 如果不包含，则返回-1
             		5. var sResult = aList01.join("-")
                        		1. 将数组以指定分隔符拼接为一个字符串
                        		2. 不影响原数组
             		6. aList01.split(2, 1, 7,8,9)
                        		1. 在数组中从第二个索引开始
                         		2. 删除1一个元素
                         		3. 增加7，8，9元素到数组
  17. 对象（object）
   18. {name: "zhangsan", age = 11}
   19. js中，数组和map都属于对象类型
   20. 属性
           1. constructor
                   1. 返回创建对象的函数实体
                           2. <pre>
                                			function employee(name,job,born)

             {
           this.name=name;
           this.job=job;
              this.born=born;
             }
             var bill=new employee("Bill Gates","Engineer",1985);
             /*返回employee函数的定义*/
             document.write(bill.constructor);
         			</pre>
       ​    2. prototype
       ​    	1. 允许向已存在的对象定义中添加属性
       ​    	2. <pre>
       ​    	employee.prototype.salary = null;
       ​        bill.salary = 20000;
       ​        </pre>    
   21. 方法
  22. 函数（function）
      ​     1. function myFunction(a, b){ return a * b}
      ​     2. 函数可以通过toString()方法转换成一个字符串
      ​     3. 值传递/引用传递
      ​     	1. js中函数调用，基本数据类型是值传递，需要接受返回值，才能获取到修改后的数据
      ​     	2. js中，复合数据类型属于引用传递
      ​     4. js特性：
      ​     	1. js中函数调用并并不关心传入参数的数量和类型 

     	<pre>
           比如定义函数 :
           function myFunc(arg1, arg2, arg3){}
           调用函数时： myFunc(1,2)
                      myFunc(1,2,3,4,5)
                      都能正常调用
           </pre>
           2. 传入参数较少时，多余的形参在函数中为undefined；
              ​	1. 为避免这种情况，可以采用手段设定默认值：
           		<pre>
                   if (arg3 === undifined){
                   	arg3 = 0
                   } 或者
                   arg3 = arg3 || 0
                   这里如果arg3未定义，则赋值为0
                   </pre>
           3. 传入参数多时，会被收集到arguments中
              ​	1. arguments是一个参数数组，是js中函数的默认属性
              	2. 其中只有前三个元素会被用到，后面的参数如果要使用，可以对arguments数组迭代取值。
       5. this关键字
       	1. js中使用this关键字，表示对象本身
       6. 函数调用
        8. 直接调用
    
             		1. 直接调用函数，this相当于window对象，就是js的当前全局作用域
       	2. js中有两个与定义方法，用于调用函数，与直接调用的差别在于，使用call/apply会改变函数的作用域，等于设置了函数体内this对象的值。
         		1. call/apply两个函数的作用是完全相同的，差别在于两者的接受的参数形式
           		2. 其实，指定的是变量作用域
             		3. web页面中全局变量属于window对象
       	2. call调用
         		1. myFunc.call(myObject, arg1, arg2...)
           		2. myFunc是函数名
             		3. myObject 是作用域，默认的作用与是全局作用域
               		4. <pre>
                 		color = 'red';
                 		document.color = 'yellow';
    
       		var s1 = {color: 'blue' };
       		function changeColor(){
       		    console.log(this.color);
      			}
       		changeColor.call(window);   //red
       		changeColor.call(document); //yellow
       		changeColor.call(this);     //red
       		changeColor.call(s1);       //blue
     ​          </pre>
    
       	3. apply调用
         		1. myFunc.apply(myObject, myArray)
           		2. myArray是一个参数组成的数组
    
     7. 变量声明是如果不使用 var 关键字，那么它就是一个全局变量，即便它在函数内定义。
     8. 闭包
     	1. 闭包可以访问上一层函数作用域中的变量，即使上一层函数已经关闭。
     	2. 一些随网页加载自动运行的程序（如加载后弹窗），使用闭包，可以有效防止因变量、方法命名冲突造成的异常。
     	3. 两种写法：
     		1. （function(){
               ​    function test(){...};
               ​    test();
               }）()   需要用括号将函数括起来，不然会有报错；
               2. !function(){
               	function test(){...};
                   test();
               }();   使用！表示闭包

  7. 空对象 null
  8. 未定义 undefined
  9. bool
   10. true
   11. false
  12. $可以作为变量标识符的首字母
  13. 获取变量类型
   14. typeof T
   15. <pre>
         	function myFunc4(arg){
           alert(arg + ": " + typeof arg)
       }
       </pre>
  16. Date
   17. 声明

         		1. var d = Date();
   18. 方法
             		1. getDate()
                        		1. 获取一个月中的哪一天（1-31）
             		2. getDay()
                        		1. 返回一周中的某一天（0-6）
             		3. getFullYear()
                        		1. 四位数字返回年份
             		4. getHours()
                        		1. 返回小时（0-23）
             		5. getMilliseconds()
                        		1. 返回毫秒（0-999）
             		6. getMinutes()
                        		1. 返回分钟（0-59）
             		7. getMonth()
                        		1. 返回月份（0-11）
             		8. getSeconds()
                        		1. 返回秒（0-59）
             		9. getTime()
                    			1. 返回时间戳（1970年1月1日至今毫秒数）
19. 注释（//）
   1. //
   2. /**/

20. 大小写敏感,没有严格缩进
21. 分号（；）
22. 代码块（{}）
23. JavaScript 会忽略多余的空格
24. 声明后未初始化的变量，value = undefined

  25. 空对象 null
26. 重复声明不报错
27. javascript中，所有数据变量都属于window对象

#### 运算符
1. 算术运算符
	1. +
		1. 数字和字符串相加，会得到一个字符串
	2. ++/--
		1. 可以作为表达式赋值给变量
2. 比较运算符
	1. === 
		1. 绝对相等（值和类型都相同）
		2. 在js中，运算符前后，会先进行隐式类型转款。
	2. !==
		1. 不绝对相等，值和类型有一个不相等则返回true

3. 三元运算符（条件运算符）
	1. voteable = (age&lt;18)?"年龄太小":"刚刚好";


#### 流程控制
1. return

2. if...else...

  1. if(condition){}else{}
3. if...else if... else...
4. switch
	1. <pre>switch(n) {
   	 	case 1:
        	code1;
        break
        case 2:
        	code2;
        break;
        default:
        	code
    }</pre>
    2. break不可以省略，不然会不断往下执行
5. for 循环

  1. for (i=0;i&lt;10;i++){}
6. while 循环

  1. while(i < 5){...}
7. do...while循环

  1. do {...} while (i&lt;5);

#### 类型转换

1. typeof操作符
	1. var T = typeof value 
	2. 返回value的数据类型
	3. typeof NaN  返回number
2. constructor 属性
  1. “john".constructor
  2. false.constructor
  3. 返回对应变量的构造函数
  4. 使用constructor进行类型断言
    1. <pre>
    	var iNum = 0;
        function isNum(n) {
          return n.constructor.toString().indexOf("Number") > -1
        }
        alert(isNum(iNum));
          </pre>
      
      2. <pre>

        	var iArray = [1,2,3,4];
        function isArr(n) {
          return n.constructor.toString().indexOf("Array") > -1
      }
      alert(isArr(iArray));
      	</pre>
3. String()

  1. 各种类型都可以转换为字符串
4. 各变量类型都拥有toString()方法
5. Number()
	1. 字符串可以转换为Number
	2. Boolean可以转换为Number
		1. Number(true) == 1;
		2. Number(false) == 0;
	3. Date转换为Number
		1. Number(New Date())
		2. 转换为时间戳
		3. d.getTime() 有同样的效果
6. 自动类型转换

  1. 进行运算操作，输出操作时，js会自动转换为对的格式
6. null
	1. 可以用来清空对象
	2. var person = null;

#### 正则表达式
1. 语法
	1. /pattern/modifiers；
	2. var pattern= /hello/i;
		1. 匹配hello
		2. i表示忽略大小写
2. 字符串方法
	1. search()
		1. n = "hello".search("el")   //使用字符串
		2. n = "hello".search(/el/i)   //使用正则表达式
		2. 返回匹配的字段起始索引
	2. replace()
		1. <pre>
				var sTmp = "hello";
                var sRest = sTmp.replace("el", world);     // 返回hwroldlo，使用字符串
                var sRest2 = sTmp.replace(/el/i, "helloworld")  // 使用正则
                </pre>
        2. 返回替换后的结果字符串
        3. 不影响源字符串

	3. match()
		1. <pre>
			"helloworldhellowrold".match(/el/g)
          </pre>
        2. 获取全部的匹配分组
   
3. 修饰符
	1. i
		1. 对大小写不敏感
	2. g
		1. 全局匹配
		2. 主要影响replace()
	3. m
		1. 多行匹配

4. 正则表达式方法
  1. test()
  	1. 检测能否匹配到，返回boolean值
  	2. 例子
  	<pre>
  		var pPatt = /el/;
           pPatt.test("helloworld");   // 返回true
  	</pre>
       3. test函数是非贪婪模式匹配，部分匹配就返回true，要全文匹配，增加^$标识
  2. exec()
  	1. 获取字符串中的第一个匹配字段
  	2. 
  3. compile()

        1. 用于改变匹配用的RegExp中的匹配字段

5. 异常处理
	1. try...catch
		<pre>
        	try{
      					adddlert("Welcome guest!");
      			}catch(err){
      				txt="本页有一个错误。\n\n";
      				txt+="错误描述：" + err.message + "\n\n";
      				txt+="点击确定继续。\n\n";
      				alert(txt);
		 	 		}
        </pre>
      
	2. throw
		1. throw errInfo;
		2. <pre>
		try {
          	var x = 23;
            if (x &lt; 40){
            	throw "太小了";
            }
          } catch(err){
          	alert(err);
          }
		</pre>
       	3. 用于自定义错误

	3. 代码调试
		1. console.log()
			1. 将信息输出到浏览器控制台
			2. console.log("运行信息")