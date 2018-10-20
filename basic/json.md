### json

1. <strong>struct 转换 json</strong>
    结构体转为Json的过程叫编组（marshaling）  
    用go语言的结构体作为json的对象，只有导出的结构体才会被编码（标识符首字母大写）  
    1. json.Marshal(struct)  
    
		    type My_struct struct{
                Name string `json:"my_name"`
                // 注意这里，原生字符串里面是一种jsontag,用来表示编组时的映射到json时的隐式转换
                // json: 冒号后面不能有空格
                Id int `json:"my_id,omitempty"`
                // omitempty 表示如果字段为空或者零值时，省略不输出
            }
     	   test_struct := []My_struct{{“zhangsan", 12}}
     	   data, err := json.Marshal()
     	   fmt.Printf("%s\n", data)
        
        注意：Marshal转换出的json中没有缩进和空格，节省空间，可读性差
       

	2. json.MarshalIndent(struct, prefix, retract)  
		函数有三个参数，源结构体，每行前缀，缩进
	    	data, err := json.MarshalIndent(move, "", "  ")
        
2. 解码json
    1. json.Unmarshl(json, &struct)
			var titles []struct{ Name string `json:"my_name"`}
            //可以通过定义接收结构体的元素，解析json中的感兴趣的数据
            
            // 这里字段包括结构体tag都要写全
            if err := json.Unmarshal(data, &titles)；err != nil {
                log.Fatalf("json unmarshaling failed: %s", err)
            }
            fmt.Println(titles)
            
    
3. 基于流式数据的转码
    1. err := json.NewDecoder(io.io.reader).Decode(&result)
    2. json.Encoder



### json 处理
####json(javaScript Object Notation)

		开发者可以使用json传输简单的字符串，数字，布尔值，也可以传输一个数组，或者一个更复杂的结构		
    	go语言中的大多数数据类型都可以转化成有效的Json文本， Channel、complex、function除外
    
#### go语言json处理函数
##### 封装为json（Encode)
1. json.Marshal(v interface{} ([]byte, error)
	1. 处理已知类型的数据结构
2. func NewEncoder(w io.Writer) *Encoder
    1. 流式文件处理（向输出流中封装json ）

![封装json.png](H:\go\markdown\basic\image\封装json.png)


		封装json的过程中，如果数据结构中出现指针，那么将会转化指针所指向的值，如果指针指向nil，则转换为null输出
        json封装过程中映射关系：
        1. bool  -->    bool
        2. float, int --> json中的常规数字
        3. string --> 将UTF-8转换为Unicode字符集字符串输出，特殊字符会被转义
        4. 数组和切片 --> JSON中的数组，[]byte类型的值会被转化为Base64编码后的字符串，slice类型的零值会被转化为null
        5. struct --> Json对象，只有首字母大写的字段会被导出，结构体字段有tag的，使用tag作为索引，没有使用字段名作为索引
        6. map --> 只有map[string]T类型才能转换为Json



##### 解封json
1. func Unmarshal(data []byte, v interface{}) error
2. func NewDecoder(r io.Reader) *Decoder


![解封json.png](H:\go\markdown\basic\image\解封json.png)

接收到json后，解封过程中，遵循如下顺序进行匹配：
1. 比对结构体字段tag
2. 比对字段名（首字母必然大写，其他字母大小写不敏感）
3. 如果Json中的字段在Go目标类型中不存在，解码过程中会丢弃该字段

解封json有两种状况：
1. 已知json结构
2. 未知json结构
	1. 使用空接口进行接收，通过类型断言进行判断
	2. 允许两种空接口模式
		1. map[string]interface{}
		2. []interface{}
	3. 转换过程中的映射关系
		1. bool  --> bool
		2. 数值  --> float64
		3. string --> string
		4. json数组 --> []interface{}
		5. json对象 --> map[string]interface{}
		6. null --> nil

![解封json_类型断言.png](H:\go\markdown\basic\image\解封json_类型断言.png)


