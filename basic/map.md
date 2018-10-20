### map

内建函数：
1. len()
2. map没有cap内建函数

#### 创建方法及特点
创建方法 | 特点
---|---
var m1 map[string]int| 未初始化内存，不能存储值
m1 := map[string]int | 已初始化内存，默认长度0
m1 := make(map[string]int)| 已初始化内存，默认长度0
m1 := make(map[string]int, 5)|已初始化内存，定义长度为5，自动增长

map 类型的键，不能使用引用类型（slice/map/func等）
借口值是可以进行比较的，可以用来做map的键


#### 初始化
	var m map[string]int = map[string]int{"zhangsan":1, "lisi":2}
    m := map[string]int{"zhangsan":1, "lisi":2}

#### 赋值
		赋值过程中，如果新map元素的key原map元素key相同，则覆盖。
        如不同，则添加
        
#### map使用
##### 遍历

![遍历map.PNG](.\image\遍历map.PNG)

使用range遍历map，有一个问题，当使用range遍历map的时候，如果同时删除/新增键值对，由于map存储是无序的，所以最终的输出结果不一致，删除/新增的元素是否会遍历出来，取决于新增/删除的时机，每次执行结果都不同。
##### 判断key是否存在

![map_值存在.PNG](.\image\map_值存在.PNG)

##### 删除
	内建函数delete函数
	delete(map, key)
    delete函数调用失败，不会报错
    
##### 作为参数
	map作为函数参数，传递引用
    