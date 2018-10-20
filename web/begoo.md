### beego
#### MVC
1. M：Model模型，数据有关操作
2. V：View视图，控制显示
3. Controller：联动M和V
4. 
![mvc-detail.png](.\img\mvc-detail.png)
[参考文档](https://beego.me/docs/mvc/)

#### 简介
1. beego是一个restful风格的框架

3. controler
	1. controler中重写的函数，如果不指定TplName参数，则默认查找view/controlername/funcname.tpl（这里指ControlerName结构体绑定的FuncName方法）,文件和目录名都需要小写
	2. obj.Data :一个用于输出数据的map
	3. obj.Ctx.WriteString("hello")用于直接输出字符串
4. module
5. view
	1. 默认支持tpl和html后缀
	2. beego.AddTemplateExt接口，可以用来调用其它后缀，还不会。。。。。
	3. 接收数据  {{.key}}   // key时data中的键
	4. 
6. 静态文件处理
	1. beego默认注册了static目录作为静态处理目录
	2. 自定义静态文件目录：
	3. 
![static目录设置.png](.\img\static目录设置.png)

### model设计
#### 概述
1. 安装orm
	1. go get github.com/astaxie/beego/orm


#### orm使用
1. 数据库设置
	1. 目前默认支持三种数据库：
		1. github.com/go-sql-driver/mysql
		2. github.com/lib/pq
		3. github.com/mattn/go-sqlite3
2. 注册数据库驱动
	1. mysql/sqlite3/postgres三种已经默认注册过，不需要设置
	2. orm.RegisterDriver("mysql", orm.DRMysql)// 显示注册驱动

3. 注册数据库
	1. orm必须注册一个别名为"default"的数据库，作为默认使用。// default是必须的
	2. orm.RegisterDataBase("default", "mysql", "root:passwd@tcp(ip:port)/database?charset=utf8"，maxIdle, maxConn)
	3. 参数说明
		1. 第一个参数：数据库的别名
		2. 第二个参数：数据库驱动
		3. 第三个参数：连接路径
		4. 第四个参数：最大空闲连接
			1. 可选参数
		5. 第五个参数：最大连接数
4. 时区设置
	1. orm.DefaultTimeLoc = time.UTC // 设置为UTC时间
	2. ORM在运行RegisterDataBase 的同时，会获取数据库使用的时区，然后再time.Time类型存取时作相应的转换。

5. 注册模型
	1. 如果使用ORM.QuerySeter进行高级查询的话，必须注册
	2. 如果只是用Raw查询和map struct，注册模型可以省略
	3. orm.RegisterModel(new(User),.....)
		1. 可以同时注册多个模型
	4. 增加前缀：orm.RegisterModelWithPrefix("prefix_", new(User))
	5. 增加后缀：orm.RegisterModelWithSuffix("_suffix", new(User))
	6. 这里增加的前后缀，体现在数据库中建表的表名
	7. 注册的模型中以驼峰命名（UserInfo），则建表时在大写字母前插入一个“_"下划线（user_info），表名全部小写

6. orm接口使用
	1. 实例化Ormer接口
		1. obj := orm.NewOrm
		2. NewOrm会同时执行orm.BootStrap（整个app只执行一次），用以验证模型之间的定义并缓存
		3. 切换数据库，查询，执行事务
7. 方法：
	1. Read(interface{}, ...string) error
	2. ReadOrCreate(interface{}, string, ...string)(bool, int64, error)
	3. Insert(interface{})(int64, error)
	4. InsertMulti(int(队列长度), interface{})(int64, error)
	5. Update(interface{})(int64, error)
	6. Delete(interface{})(int64, error)
	7. LoadRelated(interface{}, string, ...interface{})(int64, error)
		1. 载入关系字段
		2. 包含所有的rel/reverse - one/many关系
		3. 关联字段是one，则载入的是该对象的指针
		4. 关联字段是many，则载入的是对象指针的切片
		5. ![loadrelated.png](.\img\loadrelated.png)
	8. QueryM2M(interface{}, string, ...interface{})(int64, error)
		1. 这是一个接口，用来控制beego中manytomany的操作
		2. beego中的manytomay，通过新建一张关系表，管理这种对应关系
		3. 注意这里（巨坑）：
		4. M2M在使用QueryTable（）进行高级查询时，其关联的表其实是那张中间表，而不是many to many的关联表，所以，在指定条件表达式的时候，关联字段能操作的属性是中间表的字段，然后获取关联方的主键值，进而间接获取关系字段
		5. 
![M2M操作.png](.\img\M2M操作.png)

	9. QueryTable
		1. tableObj := obj.QueryTable(tableName)
		2. 传入表名，返回一个QuerySeter对象，
		3. 如果表没定义过，会引发panic异常

	10. Using(string)error
		1. 切换为其他数据库
		2. <pre>
		obj := orm.NewOrm()
        obj.Using("db1")   // 这里db1使用注册数据库连接的别名
        </pre>
        3. 不使用Using切换，则默认使用default

	11. Begin()error
		1. 事务处理过程
		2. <pre>
			obj := orm.NewOrm()
            err := obj.Begin()
            /*
            	事务中代码在这里实现
            */
            if SomeError {
            	err = obj.Rollback()
            } else {
            	err = obj.Commit()
            }
        </pre>
        3. beego orm中默认使用了自动提交，
        4. 调用Begin()函数之后，则中断自动提交，开始一个事务处理过程
	12. Commit()error
	13. Rollback()error
	14. Raw(string, ..interface{}) RawSeter
		1. 返回一个RawSeter对象，用于对设置的sql语句和参数进行操作
		2. <pre>
			obj := orm.NewOrm()
            r := obj.Raw("update user set name = ? where name = ?", "testing", "selly")
            // 使用？作为占位符，特殊点，注意
       
        </pre>
	15. Driver()Driver
		1. 返回注册数据库时使用的驱动信息
		2. 结构<pre>
			type Driver interface {
            	Name() string
          ​      Type() DriverType
            }
        </pre>
8. 调式模式
	1. <pre>
		func main(){
        	orm.Debug = ture
        }
   	 	// 调式代码是可用，生产中可能会有问题
    </pre>

#### CRUD操作
1. 前提，实例化了NewOrm(),和一个结构体对象指针
	<pre>
    	obj := orm.NewOrm()
        user := new(User)
        user := User{}
        // 两种方法都可以初始化要操作的结构体对象，区别在于new函数返回对象的指针，直接实例化返回的是对象本身，需要取值（&）后才能用
    </pre>
2. obj.Read(md interface{}, cols string)
	<pre>
    	obj.Read(user, "Id")
        // cols表示通过什么字段查询，缺省状态下默认使用“Id"字段
    </pre>
    复杂对象单个对象查找，使用One
3. ReadOrCreate(interface{}, string, ...string)(bool, int64, error)
4. Insert(interface{})(int64, error)

  1. 返回插入后的Id
5. InsertMulti(int(队列长度), interface{})(int64, error)
	<pre>
    	users := []User{
        	{Name: "slene"},
        	{Name: "astaxie"},
            {Name: "unknown"},
			...
        }
        	//初始化变量的时候，注意Id是可以省略的
        n, err := obj.InsertMulti(10, users)
        // 这里传入的是切片，切片本就是引用类型，soso~
    </pre>
6. Update(interface{})(int64, error)

  1. Update 之前需要先使用read获取完整的操作对象，再进行update操作
7. Delete(interface{})(int64, error)
	1. delete操作会对方向关系进行操作，如果on_delete设置的是默认的级联操作，删除外键记录时会将约束的对应记录都删除
	2. 删除后，自增字段的记录点不会被还原，就是说后面插入数据仍按原节奏进行

#### 高级查询
1. 简介
	1. ORM以queryseter组织查询，每个QuerySeter方法都会返回一个queryseter对象，方便链式调用
	2. 使用QueryTable获取QuerySeter对象
	3. qs := orm.NewOrm().QueryTable(tableName)

2. expr
	1. QuerySeter中使用expr查询表示字段间组合关系，
	2. 对拥有外键的查询，使用"__" 作为字段分割符，并且允许增加比较操作符进行条件查询
	3. eg__<pre>
    qs.Filter("id", 1) // WHERE id = 1
		qs.Filter("profile__age", 18) // WHERE 	profile.age = 18
	qs.Filter("Profile__Age", 18) // 使用字段名和 Field 名都是允许的
	qs.Filter("profile__age", 18) // WHERE profile.age = 18
	qs.Filter("profile__age__gt", 18) // WHERE profile.age > 18
		qs.Filter("profile__age__gte", 18) // WHERE 	profile.age >= 18
	qs.Filter("profile__age__in", 18, 20) // WHERE profile.age IN (18, 20)
	qs.Filter("profile__age__in", 18, 20).Exclude("profile__lt", 1000)
// WHERE profile.age IN (18, 20) AND NOT profile_id &lt; 1000
    </pre>

3. 比较操作符
	1. exact/iexact
		1. obj.Filter("name", "tom")
		2. obj.Filter("name__exact", "tom")
		3. 这两种写法效果相同
		
	2. contains/icontains
		1. obj.Filter("name__contains", "tom")
		2. 等价于where name like "%tom%"
		
	3. gt/gte   //大于/大于等于
		1. obj.Filter("age__gt", 17)
		2. 等价于where age > 17
		
	4. lt/lte
	5. startswith/istartswith   
	6. endswith/iendswith
	7. in
		1. obj.Filter("profile__age__in", 1,2,3,4)
		2. arr := []int{1,2,3,4}
			obj.Filter("profile__age__in", arr)
        3. 等价于where profile.age in (1,2,3,4)
	8. isnull
		1. obj.Filter("name__isnull", true)
		2. 等价于where name is null
		
	9. 以i开头表示大小写不敏感
	10. 比较操作符主要用于filter()，类比sql中的where子句

4. 高级查询接口
	1. <pre>
	type QuerySeter interface {
			Filter(string, …interface{}) QuerySeter
			Exclude(string, …interface{}) QuerySeter   // 排除条件，相当于not profile_id
			SetCond(*Condition) QuerySeter  // 自定义条件查询
			Limit(int, …int64) QuerySeter // 第一个参数是步长，第二个参数是offset，
			Offset(int64) QuerySeter  //ORM封装的操作，默认limit(1000), 这里相当于limit offset, 1000
			GroupBy(…string) QuerySeter // GroupBy("id", "age")
			OrderBy(…string) QuerySeter // OrderBy("-id", "profile__age"),前面的"-"表示desc（降序排列）
			Distinct() QuerySeter
			RelatedSel(…interface{}) QuerySeter // 将关联字段信息载入，之后filter操作才能进行，否则会取不到数据
			Count() (int64, error)
			Exist() bool
			Update(Params) (int64, error)
			Delete() (int64, error)
			PrepareInsert() (Inserter, error)
			All(interface{}, …string) (int64, error)		// 指定返回字段的时候，可以指定级联属性。就是双下划线标识
			One(interface{}, …string) error
			Values(*[]Params, …string) (int64, error)      //使用*[]orm.Params 接收，每条记录以map形式保存
			ValuesList(*[]ParamsList, …string) (int64, error)  // 使用*[]orm.ParamsList 接收，每条记录是切片形式，结果的排列与 Model 中定义的 Field 顺序一致，每个值都是以string形式保存的
			ValuesFlat(*ParamsList, string) (int64, error)   // 注意区别，返回指定一个字段，将指定字段结果集合并到一个slice中
		}
    </pre>
   
	2. 这里注意，values/valueslist/valuesFlat/all/one/都可以指定字段返回，未指明的字段以默认值完成初始化
	3. 级联模式（relatedSel()）暂时还不支持返回values，可以通过在values中指定关联字段的expr模式字段间接获取
	
#### 使用sql语句进行查询
1. 序言
	1. 使用Raw Sql查询，不需要ORM实例化
	2. 多数据库，都可以直接使用占位符？，格式化字符串，自动转换格式
	3. 查询时使用参数，支持Model Struct/Slice/Array
		1. obj.Raw("select * from day03_test_a where id in (?,?,?)", []int{2,3,4}).Values(&maps)
	4. Raw()返回一个RawSeter接口实例
2. 方法
  1. Exec()(sql.Result, error)
  	1. sql.Result对象包含两个属性方法
  	2. LastInsertId()(int64)
  	3. RowsAffected()(int64, error) // 受影响的行
  	4. <pre>
  		sR, err := obj.Raw("sql", arg...).Exec()
         	num, _ := sR.RowsAffected()
           // num就是受影响的行
         </pre>
  2. QueryRow(…interface{}) error
  	1. 使用结构体指针接收，结果是个结构体对象实例
  	2. 可以自定义个对应的结构体，接收
  3. QueryRows(…interface{}) (int64, error)

    1. 使用结构体切片的指针接收结果
  4. SetArgs(…interface{}) RawSeter
  	1. 对已有的RawSeter对象进行参数替换后，返回一个新的RawSeter对象，用于对单条语句的重复使用
  	2. 注意：不会改变原RawSeter，是一个新返回的对象。
  	3. <pre>
  	oR := obj.Raw("select * from day03_test_a where id = ?", 3)
         oR.SetArgs(7).QueryRow(&tt1)</pre>
  5. Values(*[]Params, …string) (int64, error)
  6. ValuesList(*[]ParamsList, …string) (int64, error)
  7. ValuesFlat(*ParamsList, string) (int64, error)
  8. RowsToMap(*Params, str1, str2) (int64, error)
  	1. 将结果按指定的参数映射为map，str1字段作为key, str2字段作为value。
  	2. oR2.RowsToMap(&res, "id", "column_a")
  	3. 结果是一个map，id作为key，column_a作为value。
  	4. 接收结果用一个orm.Params实例
  9. RowsToStruct(interface{}, string, string) (int64, error)
  	1. 将结果映射到一个自定义的结构体中去，会按照参数顺序匹配赋值给结构体实例，
  	2. 自定义结构体属性名称与字段名称可以不一致
  10. Prepare() (RawPreparer, error)

      1. 用于批量执行

#### sql构造器
1. 简介
	1. 用于生成合适的原生sql
	2. 然后通过orm.NewOrm().Raw(sql, &user)接收
	3. 
![sql构造器.png](.\img\sql构造器.png)

2. 完整API接口
    <pre>
    type QueryBuilder interface {
    	Select(fields ...string) QueryBuilder
    	From(tables ...string) QueryBuilder
    	InnerJoin(table string) QueryBuilder
    	LeftJoin(table string) QueryBuilder
    	RightJoin(table string) QueryBuilder
    	On(cond string) QueryBuilder
      	 	Where(cond string) QueryBuilder
    	And(cond string) QueryBuilder
    	Or(cond string) QueryBuilder
    	In(vals ...string) QueryBuilder
    	OrderBy(fields ...string) QueryBuilder
    	Asc() QueryBuilder
    	Desc() QueryBuilder
    	Limit(limit int) QueryBuilder
    	Offset(offset int) QueryBuilder
    	GroupBy(fields ...string) QueryBuilder
    	Having(cond string) QueryBuilder
    	Subquery(sub string, alias string) string
    	String() string
}
    
    </pre>

#### 参数设定
1. 简介
	1. orm后的参数字符串，首字母不能是空格
	2. 多个设置使用;分割，设置多个值则用,分隔
	3. 设置字段整体使用反引号包裹，orm键对应的值使用双引号包裹
	4. 
2. 表格
|用途|语法|备注|
|--|--|--|
|忽略字段|·orm:"-"`|
|自增|auto|需要是整型|
|主键|pk|不设置，会默认使用Id字段作为主键|
|空值|·orm:"null"`|beego中默认字段为not null|
|索引|·orm:"index"`|
|唯一性约束|·orm:"unique"`|
|设定字段名|`orm:"column(table_name)"`|这里将自定义字段名，而不是系统自动转化|
|字符串长度|·orm:"size(60)"`|设置varchar长度|
|浮点精度|·orm:"digits(12);decimals(4)"`|digits是浮点数总长度，decimals是精度|
|自动更新|auto_now/auto_now_add|用于设定时间类型自动更新，区别是auto_now每次访问都会更新，auto_now_add只有创建时才更新|
|类型|·orm:"type(datatime)"`|type中参数是db中对应的数据类型|
|默认值|·orm:"default(1)"`||

#### 表关系设置
1. one to one
	1. *Post orm:"rel(fk)"
	2. *Post orm:"reverse(one)"
2. one to many
	1. *Post orm:"rel(fk)"
	2. []*Post orm:"reverse(many)"
3. many to many
	1. []*Post orm:"rel(m2m)"
	2. []*Post orm:"reverse(many)"
4. rel_table/rel_through
	1. 针对 orm:"rel(m2m)进行的设置
	|参数名|说明|示例|
   |--|--|--|
   |rel_table|设置自动生成的m2m关系表名称|orm:"rel(m2m);rel_table(the_table_name)"|
   |rel_through|使用自定义关系表，该表中要有双方的对应关系|orm:"rel(m2m);rel_through(pkg.path.ModelName)"|
5. on_delete
	1. 设置外键约束的动作
	2. |参数|作用|示例|
	|--|--|--|
   |cascade|级联删除(默认值)|orm:"on_delete(cascade)|
    |set_null|将关系字段设置为NUll,需要该字段允许空值，即null=true| `orm:"null;rel(one);on_delete(set_null)"`
    |set_default|设置为字段默认值|
    |do_nothing|什么都不做|`orm:"rel(fk);null;on_delete(do_nothing)"`|
   
    

### beego方法
#### controller
1. this.GetString("key")   // 获取传入参数对应的值
	1. 可以获取正则路由，get/post传递的参数
2. this.Ctx.Redirect(status, url)   // 重定向
	1. 重定向和转发（渲染）
		1. 重定向（301，302）,属于浏览器行为，所以url会变为定向后url，不能传递数据
		2. 渲染（TplName)，属于服务器行为，所以浏览器显示url未变，可以传递数据


### 模板处理
#### 语法
1. 同一使用{{}}作为左右标签的标识
	1. 当前端使用angular.js时，会有冲突，可以使用下面的语句定义开始结束标签
	2. beego.TemplateLeft = "<<<"
	3. beego.TemplateRight = ">>>"

2. 使用. 访问当前位置的上下文
3. 使用$ 访问根目录的元素，所以，可以使用 $.key 访问全局变量
4. 使用$val访问模板标签中创建的变量
5. 复用go语言的类型
6. 模板中的pipeline
	1. {{. | FuncA| FuncB|FuncC}}
	2. 可以通过管道符，传递变量

7. if...else...end
	<pre>
    	{{if 表达式}}
        	code
        {{else}}
        	code
        {{end}}
    </pre>
   
8. range... end
	<pre>
    	{{range .val}}
        	code
            // 这里可以通过.号访问.val的属性
            // 使用$.val访问外面的变量
        {{end}}
        支持array, slice, map, channel
   
        或者
        {{range $idx, $v := .val}}
        	code   // 这里通过创建变量遍历参数
            // 使用$v.val 访问元素属性
        {{end}}
    </pre>
   
9. range...else...end
	<pre>
    	{{range .val}}
        {{else}}
        	// 当.val是一个空元素时，执行这里的代码
        {{end}}
    </pre>
   
10. template 
	将对应的上下文pipeline传递给模板，才可以在模板中调用
	{{ template  "模板名" pipeline}} 
    beego系统支持直接载入制定的模板文件
    {{ template "path/head.html" .}}
    
11. 注释
<pre>
	{{/*
    	code
        不支持注释嵌套
    */}}
</pre>

#### 基本函数
1. 参数传递
	1. 使用管道符
		1. {{.val|FuncA|FuncB }}
	2. 直接传递
		1. {{ FuncA .val}}
	3. 使用括号
		1. {{printf "nums is %s %d" (printf "%d %d" 1 2) 3}}

2. and
	1. {{and .X .Y .Z}}
	2. 注意判断参数，返回第一个为空的参数，否则返回最后一个非空参数

3. 条件运算符
	1. eq/ne/lt/le/gt/ge
	2. {{ if eq .v1 .v2}}   //比较.v1 == .v2
	3. {{if  lt .v1 .v2 .v3}} // .v1 < v2 || .v1 < .v3  // 支持多个参数

#### 模板处理
1. 简介
	1. beego的模板引擎采用Go内置的html/template包进行处理，默认的目录时views
	2. 通过beego.ViewsPath = "myviewpath"

2. 自动渲染	
	1. beego中默认不需要手动调用render函数
	2. 可以使用beego.AutoRender = false 设置

3. 模板标签
4. 模板数据

  1. 在Controller中使用this.Data["key"] = "key"，进行映射

5. 模板名称
	1. this.TplName = "admin/add.tpl"
	2. 不显式的指明，会引用views/controllerName/funcname.tpl

6. Layout设计
	1. this.Layout = "admin/layout.html"
	2. 函数的模板文件会嵌入到layout中然后进行渲染
	3. 需要在layout.html 中加入
		{{.LayoutContent}}标识
    4. 每个页面只能有一个{{.LayoutContent}}
   
7. LayoutSection
	1. 用于向模板中引入其他单独的模块
	2. 可以是css/javascript脚本等
	3. 可以出现多次
	4. 在模板文件中同模板数据的访问方式相同
		{{.SectionName}}
    5. 在controller中需要定义
    	<pre>
        	this.LayoutSections = make(map[string]string)
    		this.LayoutSections["HtmlHead"] = "blogs/html_head.tpl"
    		this.LayoutSections["Scripts"] = "blogs/scripts.tpl"
        </pre>

8. renderform

  1. 定义表单结构体的时候，通过formtag设定自动渲染form，不好用，样式容易走样

#### 模板函数
1. 内置模板函数
	1. compare
	2. date
	3. 还有很多，[参考文件](https://beego.me/docs/mvc/view/template.md)

2. 自定义模板函数
	1. 定义一个普通go函数
	2. 使用beego.AddFuncMap("alias", funcname)注册
	3. 使用alias进行调用
	4. 需要在beego.Run()之前调用

#### 静态文件处理
1. beego默认静态文件目录在static目录中
2. 可以使用beego.SetStaticPath("/static", "public") 注册静态文件目录，第一个参数为url，第二个为目录路径


### Controller

#### 参数设置
1. 系统配置
	1. httpport = 8080
	2. viewspath = ""
	3. autorender = false
	4. 程序中设置：beego.BConfig.AppName = "beepkg"
	5. 使用的ini格式
	6. 不区分大小写
2. 自定义配置
	1. 在配置文件中自定义：
		1. mysqluser = "root"
		2. mysqlpass = "123123"
	2. 在controller中使用
		1. beego.AppConfig.String("mysqluser)

	3. AppConfig的方法集：
		1. Set(key, val string) error
		2. String(key string) string
		3. Strings(key string)[]string
		4. int(key string)(int error)
		5. Int64(key string) (int64, error)
		6. Bool(key string) (bool, error)
		7. Float(key string) (float64, 	error)
		8. DefaultString(key string, defaultVal string) string
		9. DefaultStrings(key string, defaultVal []string)
		10. DefaultInt(key string, defaultVal int) int
		11. DefaultInt64(key string, defaultVal int64) int64
		12. DefaultBool(key string, defaultVal bool) bool
		13. DefaultFloat(key string, defaultVal float64) float64
		14. DIY(key string) (interface{}, error)
		15. GetSection(section string) (map[string]string, error)
		16. SaveConfigFile(filename string) error

3. 不同级别的配置
	1. beego配置文件支持section，通过设定不同Runmode配置实现，默认优先读取runmode指定的配置域
	2. 
![不同级别的配置.png](.\img\不同级别的配置.png)

	3. 在程序中读取：
		1. 使用“模式::配置参数名” 的形式
		2. beego.AppConfig.String("dev::mysqluser")

4. 多个配置文件
	1. 使用include关键字引入其他配置文件
	2. 
![include配置文件.png](.\img\include配置文件.png)
	3. 用户导入配置文件
	4. beego.LoadAppConfig("ini", "conf/app2.conf)
	5. 如果导入的key有冲突，后引入的覆盖之前的
	6. 

5. 环境变量配置
	1. beego配置文件支持从系统环境变量中获取配置项
	2. 格式：${环境变量}
	3. eg：
		1. runmode = "${ProRunMode||dev}"
		2. 如果存在环境变量ProRunMode则使用该变量代表的值，如果不存在则使用dev模式

6. 系统默认参数
	1. 基础配置
		1. BConfig 保存了beego所有的默认配置，可以通过beego.BConfig访问和修改
	
    
#### 路由设置
router(RESTful Controller路由)
路由设置：routers/目录下，进行路由注册

1. restful路由:
![router注册.png](.\img\router注册.png)
	2. 支持的method定义
		1. *： 包含下面所有方法
		2. get
		3. post
		4. head
		5. put
		6. delete
		7. patch
		8. options
	3. 如果同时定义了*和其他方法，优先选择具体方法定义的函数
	4. 如果自定义路由的函数无法调用，则走默认路由（默认未重写的方法返回405）
	4. [参考文档](https://beego.me/docs/mvc/controller/router.md)
	
3. 固定路由
	1. beego.Router(“/api/?:id”, &controllers.RController{})
	2. ![正则路由.png](.\img\正则路由.png)
	3. 取值方法：
		this.Ctx.Input.Param(":id")
		this.Ctx.Input.Param(":username")
		this.Ctx.Input.Param(":splat")
		this.Ctx.Input.Param(":path")
		this.Ctx.Input.Param(":ext")
        也可以使用this.GetString(key)获取

4. namespace
    1. 可以通过设置单独的namespace，配置独立的url匹配规则
    2. 步骤
    	1. 独立设置namespace变量
    	2. 在router中注册
    	3. 在namespace中可以执行判断是否执行及路由过滤
    3. eg![namespace_router.png](.\img\namespace_router.png)

	4. 接口方法
		1. NewNamespace(prefix string, funcs …interface{})
			初始化 namespace 对象,下面这些函数都是 namespace 对象的方法,但是强烈推荐使用 NS 开头的相应函数注册，因为这样更容易通过 gofmt 工具看的更清楚路由的级别关系
		2. NSCond(cond namespaceCond)
			支持满足条件的就执行该 namespace, 不满足就不执行
		3. NSBefore(filiterList …FilterFunc)
		4. NSAfter(filiterList …FilterFunc)
			上面分别对应 beforeRouter 和 FinishRouter 两个过滤器，可以同时注册多个过滤器
		5. NSInclude(cList …ControllerInterface)
		6. NSRouter(rootpath string, c ControllerInterface, mappingMethods …string)
		7. NSGet(rootpath string, f FilterFunc)
		8. NSPost(rootpath string, f FilterFunc)
		9. NSDelete(rootpath string, f FilterFunc)
		10. NSPut(rootpath string, f FilterFunc)
		11. NSHead(rootpath string, f FilterFunc)
		12. NSOptions(rootpath string, f FilterFunc)
		13. NSPatch(rootpath string, f FilterFunc)
		14. NSAny(rootpath string, f FilterFunc)
		15. NSHandler(rootpath string, h http.Handler)
		16. NSAutoRouter(c ControllerInterface)
		17. NSAutoPrefix(prefix string, c ControllerInterface)
	上面这些都是设置路由的函数,详细的使用和上面 beego 的对应函数是一样的
		18. NSNamespace(prefix string, params …innnerNamespace)
	嵌套其他 namespace
![嵌套namespace.png](.\img\嵌套namespace.png)

3. 自动匹配
	1. 自动匹配的路由通过反射获取该结构体的所有实现方法，然后进行匹配
	2. beego.AutoRouter(&controllers.ObjectController{})
		3. 系统会按照/：contorller/:method 顺序进行匹配
		4. /object/blog/2014/3/3 回去匹配objectController 中的blog方法（url中大小写不敏感）
		5. 剩下的url自动解析为参数，保存在this.Ctx.Input.Params中，如上例子，参数为map{0:2014,1:3,2:3}
	3. 新版本
		1.可以自动解析有后缀的url。
		2.如/object/blog.html   /object/blog.jpg 都会匹配到objectController 中的blog方法，然后使用this.Ctx.Input.Param(":ext")获取
4. 注解路由
	1. 注解路由，在定义contorller方法的同时，使用// @router  url [method]  的形式进行路由注册
	2. 个人觉得不值得提倡，容易混乱

####  Controller方法
1. 定义方法
	1. <pre>
		type XXXController struct{
        	beego.Controller
            // 只要实现匿名组合就行
        }
	 </pre>

2. 实现了的接口函数
	1. init(*context.Context，childName string, app interface{})

	2. prepare()
		1. 在其他函数执行之前执行，可以用来进行权限验证之类
	3. Get()
		1. 默认会返回405
	
    4. Post()
    5. Delete()
    6. Put()
    7. Head()
    8. Patch
    9. Option()
    10. Finish()
   	​	1. 在其他http Method执行后执行，可以用来回收资源
   	11. Render() error
   		1. 关闭自动渲染之后，使用

3. 提前结束运行

  1. 在函数中调用this.StopRun()，后续的动作（包括匹配Get()/finish()之类的操作）都将终止

4. 小技巧，可以在过滤器中，将ctx.Request.Method 修改实现其他方法

#### XSRF

1. 跨站请求伪造（XSRF）	

   1. 简单的说，就是非法链接通过模仿正规网站套取用户名密码，然后在非法站内直接调用API，侵害用户利益
   2. 防范通用方法，设定一个无法预知的cookie数据，然后要求所有的请求都必须带有这个cooki数据，如果该选项不匹配，则认为请求时伪造的

2. beego中开启

   1. 在配置文件中设置

      ​	

      ```
      enablexsrf = true
      xsrfkey = ssdfsdfafad  //随机字符串
      xsrfexpire = 3600   // 超时时间
      ```

   2. 在main函数入口处设置

      ```
      beego.EnableXSRF = true
      beego.XSRFKEY = "61oETzKXQAGaYdkL5gEmGeJJFuYh7EQnp2XdTP1o"
      beego.XSRFExpire = 3600  //过期时间，默认1小时
      ```

   3. 开启xsrf后，要求所有用户设置一个_xsrf的cookie，如果没有，所有的post /put/delete请求将被拒绝

3. [参考文档](https://beego.me/docs/mvc/controller/xsrf.md)

4. controller级别的屏蔽

   ```
   type AdminController struct{
       beego.Controller
   }
   
   func (a *AdminController) Prepare() {
       a.EnableXSRF = false
   }
   ```

#### 解析参数

1. 常规获取

   1. this.GetString(key string) string
   2. this.GetStrings(key string) []string
   3. this.GetInt(key string)(int64, error)
   4. this.GetBool(key string)(bool, error)
   5. this.GetFloat(key string)(float64, error)

2. Request对象获取

   1. this.Ctx.Request  
   2. this.Input().Get("id")string

3. 直接解析到struct

   1. 定义表结构体时，使用structtag标识
   2. 接收通过form-submit提交的数据
   3. ![form赋值到结构体](H:\go\markdown\web\img\form赋值到结构体.png)

4. 获取Request Body中的内容

   1. 在配置文件中设置：copyrequestbody = true

   2. 在controller中配置

      ```
      func (this *ObjectController) Post() {
          var ob models.Object
          var err error
          if err = json.Unmarshal(this.Ctx.Input.RequestBody, &ob); err == nil {
              objectid := models.AddOne(ob)
              this.Data["json"] = "{\"ObjectId\":\"" + objectid + "\"}"
          } else {
              this.Data["json"] = err.Error()
          }
          this.ServeJSON()
      }
      ```

      ​	

5. 文件上传

   1. 需要在form中增加enctype="multipart/form-data"属性

   2. beego默认缓存为64M,通过beego.MaxMemory = 1<<22 调整（配置文件中为MaxMemory = 1 << 22）

   3. beego方法

      1. GetFile(key string)(multipart.File, *multipart.FileHeader, error)

         1. 根据上传文件的name，获取相应的信息，第一个返回值是一个文件对象，第二个返回值保存文件的信息

      2. SaveToFile(fromfile, tofile string)error

         1. 第一个参数：上传文件的name
         2. 第二个参数：代码保存路径



6. 数据绑定

   1. 使用this.Ctx.Input.Bind(&id, "id")将请求url中的参数直接绑定到指定对象上

      第二个参数就是url中参数的键

#### Session控制

1. 内置的session模块：支持memory，cookie，redis等

2. beego开启功能：

   1. main函数中：beego.BConfig.WebConfig.Session.SessionOn=true

   2. 配置文件中：

      sessionon = true

3. 常用方法：

   1. SetSession(name string ,value interface{})
   2. GetSession(name string)interface{}
   3. DelSession(name string)



#### 过滤器

1. 过滤器函数：
   1. beego.InsertFilter(pattern string, position int, filter FilterFunc, params ...bool)
   2. 三个必选参数，一个可选参数
   3. 参数1：路由规则，可以使用“*”全匹配
   4. 参数2：position执行Filter的地方，五个固定参数：
      1. BeforeStatic 静态地址之前
      2. BeforeRouter 寻找路由之前
      3. BeforeExe 找到路由之后，执行controller之前
      4. AfterExec 执行完Controller逻辑之后的过滤器
      5. FinishRouter 执行完逻辑之后执行的过滤器
   5. 参数3： filter 函数：type FilterFunc func(*context Context)
   6. 参数4：params
      1. returnOnOutput（默认true），是否允许有输出就跳过其他filter，默认如果有输出，就不再执行后续的filter
      2. 是否重置filter的参数，默认是false，就是不同级别的正则匹配时，同一变量（如:splat）的值是否被后续匹配规则覆盖
2. 