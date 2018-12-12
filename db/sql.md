### 数据库
#### 数据模型：
1. 数据结构
  1. 数据类型
  2. 数据之间关系
2. 数据操作
3. 数据完整性约束
   1. 非空约束
   2. 唯一性约束
   3. 外键
   4. 主键
   5. 检查性约束

#### 常见数据库
1. 关系型数据库
	1. 二维表组成
		2. 关系：一个二位表就是一个关系
		3. 元组：二维表中的一行，即表中的记录
		4. 属性：二维表中的一列，用类型和值表示
		5. 域：每个属性的变化范围
	2. 关系型数据库规范化
		1. 第一范式（1NP）
			1. 数据表中的字段都是单一的，不可再分的原子属性
		2. 第二范式（2NP）
			1. 满足第一范式是第二范式的前提
			2. 数据库表中的每个记录行必须可以被唯一区分
			3. 这里的唯一性属性被称为主键
				1. 可以是多字段组合关键字--- 符合主键，但这种就不符合第二范式
			4. 关系中每一个非主关键字段都完全依赖与主关键字段，不能只部份依赖于主关键字段
			
		3. 第三范式（3NP）
			1. 传递函数依赖
			2. 一个关系中不包含已在其他关系已包含的非主关键字信息
			3. 一个关系中，唯一性表示字段只能有一个，所有非关键字段只能依赖于主关键字段，不能存在其他可唯一标定该字段的字段
			4. 非主键列互不依赖
		4. 第四范式（4NP）
			1. 禁止主键列和非主键列一对多关系不受约束
			2. 这些非主属性不应该有多值
		5. 第五范式（5NP）
			1. 将表分割成尽可能小的块，为了排除在表中所有的冗余.
2. 数据库基本操作
	1. show variables like "%char%";
	2. show charset;
	3. create database if not exists database_name;
	4. show create database database_name;
	5. show databases;
	6. system clear;
	7. use gotest;
	8. alter database gotest character set gb2312;
	9. drop database if exists database_name;
		1. drop table tb1, tb3;
	10. create table if not exists tb1(id int auto_increment primary key, username varchar(30) not null, passwd varchar(30) not null, createtime datetime);
	11. desc tb1;
		1. describe tb1;
	12. show columns from tb1 from gotest;
		1. show columns from gotest.tb1;
	13. alter table tb1 
		1. alter table tb1 change user username varchar(50)
		2. alter table tb1 modify user varchar(60);
		3. alter table tb1 add email varchar(48);
		4. alter table tb1 drop user;
		5. alter table tb1 rename to tb3;
		6. rename table tb1 to table1;
		6. alter table tb1 rename column user to username;
		4. 使用alter修改列，前提是将表中所有数据都删除
	14. create table tb1 like tb2;
		1. create table tb3 as select * from tb1;
	15. insert into table_name (line...) values(value...),(value...),(value...)...
	
		1. 可以插入部分，也可以全部插入
		2. 可以插入多条
		3. 也可以insert into tb4 set user="zhangsan', id=1, passwd="123123";
		4. insert into tb2 select * from tb4 where between 123 and 500;
		5. insert ignore into tb4 values(3, "wusun");   --使用ignore关键字，忽略重复数据，存在则跳过
		
	16. select distinct passwd from tb4; 
		1. distinct关键字用于去重
	17. where 关键字：
		1. select line from table where line 运算符 value
		2. 可用运算符：=，<>, >, <, >=,<=,between，like
		3. insert into tb2 select * from tb4 where between 123 and 500;
		4. where 条件表达式，解析的时候从右往左解析，所以调优时，尽量将可以断言的判断式写在右边。
		5. eg： select * from b where a < b and a< c ; a < c应该是最容易判断真假的值
		
	18. or and关键字
		1. 可在where子语句中把两个或多个条件结合起来
		2. select * from tb4 where passwd > 500 and username = "zhangsan";
	19. update
		1. update tb4 set passwd = "changed", id=333 where username = "zhangsan";
		
	20. delect 
		1. delete from table where username = "zhangsan";
		2. delete * from table;
		3. delete from table;
	21. truncate table
		1. 删除表中所有数据，表结构，列，约束，索引保持不变。新行标识使用的计数器重置为种子，保留标识计数使用delete
		2. 与delete相比，truncate全表删除（不能进行条件控制），不写入日志，效率高，数据不可恢复。
		3. truncate使用系统和事务日志资源少
		4. delete 语句每次删除一行，并在事务日志中为所删除的每一行记录一项，truncate 通过释放存储表数据用的数据页来删除数据，并且旨在事务日志中记录页的释放
		5. 由于truncate不记录在日志中，所以他不能激活触发器，有外键约束的表不能使用这个操作进行清空
		6. truncate属于DDL语言，DELETE属于DML语句
	22. order by
		1. 使用列名/别名
			1. select * from hero order by leaderId, salary; 
			1. 根据参数列表中字段，依次排序（asc关键字升序）
		2. select * from hero order by salary desc;
			1. desc关键字用于反向排序（由大到小）
		3. null 认为是最小的值
		4. 使用序号排序：
			1. select name, age , sal from emp order by 3;(以select后第三个参数列进行排序，就是sal)
		5. 使用表达式排序
			1. select name, age, sal from emp order by sal * 12 + age;
	23. escape 指定转移符
		1. select * from hero where like "%#_%" escape '#'; (意思指定#为转义符，进行查找)

### 子句操作
1. where 子句
  1. 用于定位记录
  2. 使用where子句，比较运算符和like语句中不区分大小写，可以使用binary关键字指定区分大小写
  	1. select * from tb1  where binary  user = "ZHANGSAN";

2. having 子句
  1. 类似where操作，
  2. group by后面接条件语句，必须使用having
3. in
  1. select * from hero where job in ("上忍", "中忍") order by salary;
  2. select * from hero where job not in ("上忍", "中忍") order by salary;
  	1. not in结果集中不包括NULL。
  	2. 参数列表中不能有null,如果出现null,则一条都查不到(这是因为从右往左断言，最后一条如果是null,相当于or != null，直接判断为false，不再往左走，最后结果为空)
  3. select * from hero where heroId in (select distinct leaderId from hero);
  	1. 子查询
4. between and
  1. select * from hero where salary between 7000  and 10000;
  	2. 7000和10000都能取到，闭区间
  	3. 两个参数的顺序必须是从小到大
  	4. between and 数字和字母都可以
  4. not between 表示不在区间里
5. limit
  1. select * from hero limit 5;
  	1. 显式前5条，相当于limit 0, 5
  2. select * from hero limit 5,10;
  	1. 第六条开始，显式10条
  3. 索引从零开始
6. 通配符
  1. %：零个或多个字符
  2. _: 代表任意一个字符
  5. 通配符必须与like语句同时使用

7. Null
  1. select * from  hero where commission is null;
  2. select * from  hero where commission is not null;
  3. 判断一个字段是否为NULL值，不能使用普通的比较运算符，不报错，但也没有返回值

8. alias   --   as 关键字
  1. select h.salary from hero as h where h.job in ("上忍");   -- 为表设定别名
  2. select h.salary as sa from hero as h where h.job in ("上忍");   --- 为字段设定别名

9. group by
  1. select job from hero group by job;
    1. select 后只能出现，group by 后面出现的分组关键字，和相关分组内的算术计算值（内置函数）
    2. select deptId, avg(salary) as avg_salary from hero group by deptId;
  2. select leaderId, job from hero group by leaderId,job;
    1. 多元素分组，先按照后面的元素分组
  3. select job, group_concat(heroName) from hero group by job;
    1. 获取分组后包含的其它元素
  4. select 后面出现的字段，如果不出现在聚合函数中，则必须出现在group by子句中，如果聚合函数/group by子句中都没有出现，则报错。
  5. group by 后面接条件语句，必须使用having

10. isnull 函数
  1. isnull(expr) 如果expr是null，返回1， 如果不是返回0。
11. join 和key
   1. inner join(只显示交集)
     1. select h.heroName, h.job, d.deptName from hero as h, dept as d where h.deptId = d.deptId;    -- 引用多张表，
     2. select h.heroName, h.job, d.deptName, h.salary from hero as h inner join  dept as d on h.deptId = d.deptId order by h.salary;
       ![交集联表](H:\go\markdown\db\image\交集联表.png)

   2. left join（以左表为基础，联表）
     3. LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行
     1. select * from tb1 left join tb2 on tb1.id = tb2.id;
     2. select * from tb1 left join tb2 on tb1.id = tb2.id where tb2 is null
     ![左联表](H:\go\markdown\db\image\左联表.png)
   3. right join（以右表为基础，联表）
     1. select * from tb1 right join tb2 on tb1.id = tb2.id;
     2. select * from tb1 right join tb2 on tb1.id = tb2.id where tb1 is null;
   4. left join union right join（整表，或者空心整表）
     1. select * from tb1 left join tb2 on tb1.id = tb2.id union select * from tb1 right join tb2 on tb1.id = tb2.id;
       ![全数据联表](H:\go\markdown\db\image\全数据联表.png)

     2. select * from tb1 left join tb2 on tb1.id = tb2.id where tb2 is null union select * from tb1 right join tb2 on tb1.id = tb2.id where tb1 is null;
     ![交集之外元素的总合](H:\go\markdown\db\image\交集之外元素的总合.png)

12. union
    1. union 操作用于合并两个或多个select语句的结果集
    2. union内部的select语句，必须拥有相同数量和顺序的字段，列必须有相似的数据类型
    3. 返回结果的表头由最前面的select语句决定，后面select语句的结果，只要数据类型相似，按顺序添加到结果集中
    4. union默认会合并书面值相同的结果
    5. 使用union all 可以显示全部结果，不合并
    ![union_sql](H:\go\markdown\db\image\union_sql.png)

13. ifnull
    1.  select a.heroName as "英雄", ifnull(b.heroName, "boss") as "老板".from hero as a left join oin hero as b on a.leaderId = b.heroId;
    2.  ifnull(a, b)如果a不为null，返回a,如果为null，返回b 

### 数据约束（通过create table）
1. NOT NULL
	1. 字段强制不接受NULL
	2. 不向该字段添加值，该记录无法添加
2. UNIQUE
	1. 唯一性标识
	2. primary key 拥有自动定义的unique
	3. 每个表中只能拥有一个primary key,可以拥有多个unique
	4. unique又称为候选键，如果没有设定primary key， 则第一个unique key将作为主键存在
	4. 定义唯一性约束
		1. 定义字段的同时定义unique key，以字段名作为每个key的名称![定义地段同时定义unique约束.png]
![定义地段同时定义unique约束.png](.\image\定义地段同时定义unique约束.png)

		2. 单独用unique函数定义unique key，使用参数中第一个字段名作为key的名称
![单独定义unique约束.png](.\image\单独定义unique约束.png)

		3. 自定义key的名称
![自定义命名约束定义.png](.\image\自定义命名约束定义.png)

		4. 为已存在的表添加unique key
			1. alter table tb1 add unique(username);
			2. create unique index idx_emp2 on emp2;

		5. 删除唯一性约束(使用键名称作为标识)
![删除unique_key.png](.\image\删除unique_key.png)

			2. drop index idx_emp2 on emp2;
	
3. PRIMARY KEY
	1. primary key属性
		1. unique 属性
		2. not null
		3. 表中唯一
	2. 定义
		1. 定义字段的同时定义主键
			1. 定义字段中使用 primary key 关键字
		2. 单独定义主键
			1. primary key (line_name)
			
		3. 为主键命名，单独定义
			1. constraint pk_name primary key(line_name)
		4. 新增主键
			1. alter table tb1 add primary key(line_name)
		
		5. 由于主键在表中拥有唯一性，所以主键命名无意义？（show create table 看不到）
	3. 删除主键
		1. alter table tb1 drop primary key
4. FOREIGN KEY
	1. 一个表中的foreign key指向另一个表中的primary key
	2. 外键的要求：
		1. 插入数据时，可以是指向的主键中存在的记录，也可以为null，不可以是指向的主键中不存在的值
		2. 
	2. 定义：
		1. 建表时创建
![定义表同时定义foreign_key.png](.\image\定义表同时定义foreign_key.png)

		2. 后添加
			1. alter table tb2 add constraint fk_tb1 foreign key (f_id) references tb1(id);
			2. 
![已存在表_新增外键.png](.\image\已存在表_新增外键.png)

	3. 新增外键
		1. alter table tb3 add constraint fk_tb1 foreign key(f_id) references tb1(id);
	4. 删除外键
		1. alter table tb3 drop foreign key fk_tb1;
5. CHECK
	1. mysql中只对check约束进行分析，实际执行时会忽略
	2. 目前，也就是个备注的作用
	
6. DEFAULT
7. AUTO_INCREMENT
	1. 一张表中只允许一个自增字段存在，需要设置未primary key
	2. 使用alter table tb4 auto_increment = 10设定初始种子
	3. 使用delete删除数据，不会影响自增量的值
	4. 使用truncate清空数据，自增量值初始化为1
8. COMMENT


### 聚合函数

	值为null的记录不统计
1. count
	1. 值为null的记录不统计
2. sum
	1. select sum(commission) from hero;
3. avg
	1. select avg(salary) from hero;
4. max
5. min
6. 聚合函数有滤空的功能，结果中不包含字段值为null的记录



### 子查询（子查询的结果可以当作一张表对待）
1. select * from hero where salary < (select min(salary) from hero where deptId = 30);
2. select * from hero where heroId  in (select distinct heroId from hero where leaderId is not null);
	1. in 后面的参数列表不能有null，否则返回空结果集
3. 子查询出现在from后（作为查询源表，需要使用别名赋予表明，否则报错）
	1. select * from (select name, age from emp); 报错
	2. select * from (select name, age from emp) as tmpA; 不报错
3. 子查询通过括号决定执行顺序
4. any
	1. select * from hero where salary > any (select salary from hero where deptId = 30) order  by deptId, salary desc;
	2. 结果集中的任何一个
5. all
	1. select * from hero where salary > all (select salary from hero where deptId = 30) order  by deptId, salary desc;
	2. 结果集中的所有
6. exists
	1. select * from hero where exists (select * from dept where deptName = "总部");
	2. 类似if判断，如果后面条件为真，执行前面的查询，如果为假，则不执行

7. regexp
	1. select * from hero where heroName regexp '[日]';
		1. []标识匹配的内容
	2. select * from hero where job regexp '[^上忍]';
		1. 这里取反，只能取到完整的字符，即完全匹配“上忍”的记录
	3. select * from hero where heroName regexp '^猿飞|^春';

### 函数
	使用select操作函数

1. floor() 向下取整
2. ceiling() 向上取整
1. round(a,b) 四舍五入操作
	1. b为保留位数，正值表示小数点后几位，负值表示小数点前几位
	2. 默认b为0

2. rand()    //
	1. 获取随机数，不需要设置随机数种子
	2. 获取0-1之间的随机小数
	3. select floor(rand()*100),ceiling(rand()*100),round(rand()*100);

5. pi()
	1. 获取RI值
6. sqrt()   // 求平方根
7. insert(s1, x, len, s2)
	1. 所以计数从1开始
	2. 将s1 字符串中从x开始，长度len的内容替换为s2
8. upper(s)
9. lower(s)
10. left(s, n)
	1. 从前往后取n个字符
11. right(s, n)
	1. 从后往前取n个字符
12. concat(s1, s2, s3...)
	1. 将参数列表中字符串拼接成一个，返回
13. trim()    // 去除首位空格
14. rtrim()
15. ltrim()
16. substring(s, x, y)
	1. 获取子字符串，s：源字符串，x起始索引，y长度
17. reverse(s)
	1. 反转字符串
18. field(s1, s2, s3...)
	1. 返回s2开始的参数列表中，与s1相同的元素的索引，参数列表中索引从1开始，没找到则返回0）

19. locate(s1, s2)
	1. 在s2中查找s1字符串，返回匹配元素在字符串中的索引起始值，没有匹配到返回0
	2. 字符串中
20. position(s1 in s2)
	1. 与locate作用相同，返回s2中s1的起始索引
21. instr(s2, s1)
	1. 与locate作用相同，返回s2中s1的起始索引

22. curdate(), current_data()
	1. 获取当前日期

23. curtime(), current_time()
	1. 获取当前时间

24. now()    // 获取当前日期和时间
25. datediff(t1, t2)
	1. t1, t2 是两个date类型数据yyyy-mm-dd
	2. 返回t1 - t2的结果，即两者之间相差多少天（注意顺序）

25. adddate(d1, n)
	1. d1 是date类型， n是整型
	2. 返回一个d1,n天之后的日期（date类型）
	3. 默认使用天作为间隔单位
	4. 特殊：select adddate('2018-08-08', interval '1 3' year_month);

26. subdate(d1, n)
	1. d1 是date类型， n是整型
	2. 返回一个d1,n天之前的日期（date类型）
	3. 默认使用天作为间隔单位

27. version()
28. connection_id()
	1. 查看连接id

29. database()
	1. 查看当前所在的数据库
30. user()

31. charset(s),collation(s)
	1. 获取字符串字符集，和排序规则
	2. select charset("aaa"), collation("aaa");
	3. show charset  查看数据库支持的字符集

32. md5(s)
	1. 进行MD5加密
	2. select md5("hello");

33. sha(s)
	1. 进行sha加密
	2. select sha("hello");
	1. 进行sha加密

35. format(x, n)
	1. 格式化输出数字，xxx,xxx,xxx.xxxxxx
	2. 第二个参数为保留位数
	2. select format(pi(), 3)
		1. 查看pi，保留三位小数

36. convert(t1, T)
	1. 将t1转换成T类型
	2. select now(), cast(now() as date), convert(now(), date);
	3. 两个都是用来进行类型转换的，语法稍有不同
	4. 可以转换的类型是有限制的：
		1. 二进制：binary
		2. 字符型：char
		3. 日期：date
		4. 时间：time
		5. 日期时间型：datetime
		6. 浮点数：decimal
		7. 整数：signed
		8. 无符号整数：unsigned

### 流程控制
1. if()
	1. select heroName, commission, if(commission, “yes”, “no”) from hero;
	2. 如果commission不为null， 输出第二个参数yes，
	3. 如果commission为null，则输出第三个参数no
2. ifnull()
	1. select heroName, commission, ifnull(commission, "no") from hero;
	2. 如果commission不为null，输出commission值
	3. 如果commission为null，输出第二个参数no
3. case when
	1. 类似if elseif  else 语句的流程控制
	2. 
![case流程控制.png](.\image\case流程控制.png)




### 笛卡儿积
1. 联表查询时，底层出现一个笛卡儿积临时表
2. 列数 = a列数 + b列数
3. 行数 = a 行数 * b行数
4. 这个临时表中有费数据，所以增加条件语句进行筛选。
5. 需要n-1


### 分类
#### DML
1. 数据操作语言
2. 增删改查
#### DDL
1. 数据定义语言
2. create,drop, truncate
3. 有隐式提交（系统自动进行commit）
#### DCL
1. 数据控制语言
2. grant，revoke


### 索引
1. 引言：
	1. 提高查询/更新的效率
	2. 索引用于快速查找出在某个列中有一特定值的行，不适用索引，mysql必须从第一条记录开始读完整个表，知道找出相关的行，表越大，查询数据花费的时间越多。
	3. 索引是有序的。