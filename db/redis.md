### redis
#### 连接
1. redis-cli --raw
	1. 默认连接不能正常显示中文字符，使用raw参数

### 数据库操作
#### string
##### 值操作
1. select num
	1. 选择数据库
	2. redis默认创建了0-15,16个数据库

2. set
	1. set key value  // 设置字符串
3. mse
	2. mset key1 value1 key2 value2  //都是以字符串形式保存
3. get
	1. get key
4. mget
	1. mget key1 key2
4. setex
	1. setex value timeout value
	2. 设置有失效时间的字符串，timeout单位是秒

5. append
	1. append key prefix
	2. 在字符串后追加字符串
	3. 返回值：追加后的字符串长度


##### 键操作
1. keys
	1. keys Pre*  //通配查找Pre开头的键

2. exists
	1. exists key1 key2
	2. 存在返回1， 不存在返回0

3. del
	1. del key1 key2
	2. 返回成功删除的个数，无效的删除不报错
4. expire
	1. expire key timeout
	2. 秒为单位，设置超时时间

5. ttl
	1. ttl key
	2. 查看剩余超时时间

6. type
	1. type key
	2. 返回键的类型

#### hashes
##### 值操作
1. hset
	1. hset key field value
	2. 所有的value还是string类型保存
	3. 新版redis支持hset key field1 value1 field2 value2
2. hget
	1. hget key field
3. hmset
	1. hmset key field1 value1 field2 value2

4. hgetall
	1. hgetall key
	2. 获取全部属性，属性值

5. hkeys key
	1. 获取全部的属性名列表
6. hvals key
	1. 获取全部的属性值列表
7. del 
	1. del key
	2. 删除整条记录
8. hdel key field
	1. 删除某个属性


#### list
##### 值操作
1. lpush
	1. lpush key value1 value2 value3
	2. list数据插入和读取按照FILO规则，第一个读取到的是最后插入的值，索引0表示最后一个插入的元素
	3. push操作其实相当于追加，不会覆盖之前的数据

2. rpush
	1. rpush key value1 value2
	2. 插入的值按照FIFO规则，索引0表示第一个插入的值

2. lrange
	1. lrange key startIdx endIdx
	2. 获取列表中的值（一个区间）
	3. 支持-1索引
	4. 结果是一个闭区间，就是说前后都包括

4. lrem
	1. lrem key count value
	2. count 表示使用要删除值的数量，正数表示从前往后删，负数表示从后往前删
	3. startidx= -1 表示从后往前删除

5. linsert
	1. linsert key before value1 value2 // value1之前插入value2
	2. linsert key after value1 value3 // value1 后面插入value3


#### set
##### 值操作
1. sadd 
	1. sadd key value1 value2 value3
	2. 集合中不会有重复数据，重复插入无效

2. smembers
	1. smembers key
	2. 无序排列

3. srem
	1. srem key value


#### Sorted Sets
1. 与Sets比较，主要是增加了一个权重值，方便排序
##### 值操作
1. zadd
	1. zadd key weight1 value1 weight2 value2 weigh3 value3
	2. 对同一元素重复插入，会更新相应的权重

2. zrange
	1. zrange key startidx endidx
	2. end可以使用-1

3. zrangebyscore
	1. zrangebyscore key startweight endweight
	2. 根据权重查询

4. zrem
	1. zrem key value

5. zscore
	1. zscore key value
	2. 获取值对应的权重值

6. zremrangebyscore
	1. zremrangebyscore key startweight end weight
	2. 根据权重范围删除记录

### 集群
#### 主从配置
1. slaveof host port
2. 
