### 文本和html模板

一个模板是一个字符串或一个文件，里面包含了一个或多个由双花括号包含的{{action}}对象  



	import html/template
    // html/template   和 text/template 都提供template 模板包
    // 两者使用同样的API和模板语言，html/template 增加了一个将字符串自动转义的特性
    // 避免输入字符串和HTML/JavaScript/CSS/URL语法产生冲突，防止跨站之类攻击
    // 如果想抑制这种自动转义，可以使用template.HTML（信任的HTML字符串）
   
	const temp1 = `name: {{.Name}}
    Id: {{.Id | sub}}    //  '|' 作用等同于Linux 中管道符
    {{range .Loop_test}}--------------------
    sub_name： {{ .Sub_name}}
    id: {{.Sub_id}}
    {{end}}`
    
    func sub(a int)int{
    	return a /10
    }
    
    type Sub_temp struct {
    	Sub_name string
        Sub_id int
    }
    
    type temp struct {
    	Name string
        Id int
    }
    
    func main() {
    	obj := temp{ "zhangsan", 123, []Sub_temp{{"lisi",333},{"wangwu",383}}}
        // 生成模板的输出需要两个处理步骤，
        // 1. 分析模板并转为内部表示   一般只需要执行一次
        // 2. 基于指定的输入执行模板
        
        report, err := template.New("report").Funcs(templat.FuncMap{"sub", sub}).Parse(temp1)
    }
    if err := report.Excecute(os.Stdout, obj);err != nil {
    	log.Fatal(err)
    }
    
    
    