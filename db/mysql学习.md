### mysql

#### 实用sql
1. show index from table_name;
	1. 显示数据表的详细索引信息，包括primary key

2. show table status from  database_name;
	1. 数据库中表的性能及统计信息

3. show table status from database_name like "%hero%";


#### 事务
1. mysql中只有使用了Innodb数据库引擎或表才支持事务
	1. MyISAM 不支持事务
2. 四个特性(ACID)：
	1. 原子性（Atomicity)
	2. 稳定性（Consistency）
	3. 隔离性（Isolation）
	4. 可靠性（Durability）
		1. innodb_flush_log_at_trx_commit 决定什么时候把事务保存到日志中。
		
3. 事务用来管理insert, delete, update 语句。
4. 隔离性
	1. 风险：
		1. 脏读
		2. 不可重复读
		3. 幻影数据
	2. 实现：
		1. 事务的隔离通过锁机制实现，不同于MyISAM，inodb采用了细粒度的行锁定
		2. innodb的锁通过锁定索引实现
		3. 如果查询条件中有主键，锁定主键，如果有索引，先锁定对应的索引再锁定主键
			1. 可能造成死锁，一个事务锁定了主键，另一个锁定了索引
		4. 如果没有索引和主键，则锁定全表
	3. 四个隔离级别
		1. read uncommited  // 读未提交
			1. 不隔离select
			2. 其他事务未commit的修改，也可查询
			3. 脏读，幽灵数据，不可重复读
			4. READ UNCOMMIT不会采用任何锁。
		2. read commited   //读已提交
			1. 考虑其他事务的commit操作
			2. 同意事务中select操作可能有不同结果
			3. 数据的读是不加锁的，但是数据的写入、修改、删除加锁，避免了脏读
		3. repeatable read（默认）   // 可重复读
			1. 不考虑其他事务的commit
			2. 同一事务的select返回相同结果（本事务中无修改）
			3. 数据的读、写都会加锁，当前事务如果占据了锁，其他事务必须等待本次事务提交完成释放锁后才能对相同的数据行进行操作。
		4. serializable   // 串行化
			1. 事务串行而不并发，通途量很低
	4. 设置隔离级别
		1. my.cnf配置文件
			1. transaction-isolation = READ-COMMITTED
		2. 动态设置
			2. SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;    // 会话级别
			3. SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED；    // 全局级别

5. 原子性、持久性、稳定性
	1. 通过redo、undo日志文件实现
	2. redo,undo文件都会有一个缓存redo_buf, undo_buf日志文件
	3. 数据库文件有一个缓存data_buf
	
    4. 事务开始后，未修改之前的数据，保存到undo_buf中
    5. 修改后的数据保存到redo_buf中
    6. 事务中涉及的数据会保存在data_buf中，
    7. 在事务提交之前，缓存文件会写入磁盘，顺序：undo_buf --> redo_buf --> data_buf

6. 事务操作
	1. autocommit参数
		1. 置为0，挂起自动提交，显式的进行commit之前，都将作为一个事务存在
		2. 置为1，每条语句都作为单独的事务进行自动提交
	2. BEGIN,COMMIT,ROLLBACK
		1. BEGIN 执行后，挂起自动提交。


#### 存储过程
1. 定义
	1. 一组可编程SQL语句集，经编译创建并保存在数据库中，通过存储过程名称进行调用
	2. 创建的存储过程保存在数据库的数据字典中

2. 优点
	1. 简化sql调用
	2. 批处理（sql+循环），减少了流量，”跑批“
	3. 统一接口，确保数据安全

3. 创建
	<pre>
    	CREATE
    		[DEFINER = { user | CURRENT_USER }]
　			PROCEDURE sp_name 		([proc_parameter[,...]])
    		[characteristic ...] routine_body

		proc_parameter:
   			[ IN | OUT | INOUT ] param_name type

			characteristic:
    		COMMENT 'string'| LANGUAGE SQL| [NOT] DETERMINISTIC | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA } | SQL SECURITY { DEFINER | INVOKER }

		routine_body:
			Valid SQL routine statement

		[begin_label:] BEGIN
　　			[statement_list]
　　　			……
		END [end_label]
        </pre>
4. [参考文档](https://www.cnblogs.com/aspwebchh/p/6652855.html)