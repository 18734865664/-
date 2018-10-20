###字符串转换
stings.Replace() 
如果使用小于0的值，默认替换所有

### rune类型
go语言中定义一个rune类型，等同于int32，定义该类型是因为，go语言底层使用utf-8编码处理字符串语句，每个汉字占3个字符，含有中文的字符串使用len(函数得出的结果默认是)

    // rune is an alias for int32 and is equivalent to int32 in all ways. It is
	// used, by convention, to distinguish character values from integer values.

	//int32的别名，几乎在所有方面等同于int32
	//它用来区分字符值和整数值

	type rune = int32


 go语言中string底层是通过byte数组实现的，中文字符在unicode下占2个字符，在utf-8编码下占3个字符，而golang默认编码正好使用utf-8
 
byte 和 rune的区别

    byte 等同于int8，常用来处理ascii字符
    rune 等同于int32，常用来处理Unicode或者utf-8
 
####补充 int类型

    	Int16, 等于short, 占2个字节. -32768 32767
		Int32, 等于int, 占4个字节. -2147483648 2147483647
		Int64, 等于long, 占8个字节. -9223372036854775808 9223372036854775807
		Int8 ，占用1个字节
        
        
#### 补充fmt
	
    当为一个复杂类型定义一个String方法后，fmt包就会特殊对待这种类型的值，调用该方法输出，这样可以让这些类型打印时显得更友好，而不是直接打印原始值。
    fmt会直接调用用户定义的String方法
    这种机制依赖于借口和类型断言
    
    
![fmt_String.png](.\image\fmt_String.png)


#### 方法
strings.Split(string, sep) []string
strings.Field(string) []string  // 默认以空格拆分字符串

// 是否有指定后缀
strings.HasSuffix(s, suffix string) bool
// 是否有指定前缀
strings.HasPrefix(s, prefix string) bool

