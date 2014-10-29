----------------------------------------------------------------------------------------------------------
第一章 简介
    Redis： 开源的高性能键值对数据库。
    Redis支持的键值数据类型：字符串类型，散列类型，列表类型，集合类型，有序集合类型。
    Redis可以为每个键设置生存时间，到期自动删除，始于作为缓存系统；列表类可以用来实现队列，支持阻塞式读取，易于实现高性能的优先级队列；支持“发布/订阅”的消息模式。
----------------------------------------------------------------------------------------------------------
第二章 准备
    安装Redis：1. 下载redis-stable.tar.gz
        2. tar xzf redis-stable.tar.gz
        3. cd redis-stable
        4. make
    启动： redis-server  //默认使用6379端口，可用“redis-server --port 6379” 指定启动端口。
    关闭： redis-cli shutdown   //Redis收到SHUTDOWN命令后，先断开所有客户端连接，然后根据配置执行持久化，最后完成退出。
    Redis命令客户端： redis-cli -h 127.0.0.1 -p 6379   //连接
        redis-cli   //使用默认参数（同上）连接
----------------------------------------------------------------------------------------------------------
第三章 入门
	命令(不区分大小写)：
		KEYS pattern： 查看数据库中的键；
			？：一个字符
			*：0个或多个字符
			[]：匹配其中的任一个字符
			\x：转义，如"\?"匹配问号
		EXISTS key：判断键key是否存在，存在返回1,否则返回0.
		DEL key [key ...]：删除一个或多个键，返回值为删除键的个数(删除命令不支持通配符)
		TYPE key：获取键值的数据类型，返回值可以为string, hash, list, set, zset(有序集合)
----------------------------------------------------------------------------------------------------------
	字符串：能存储任何形式的字符串，包括二进制数据。允许存储的最大容量是512MB。
		SET key value：赋值
		GET key：取值
		INCR key：key递增1(键不存在时默认键值为0, 所以第一次递增后为1.)
		INCRBY key increment：key的值增加increment
		DESC key：key递减1
		DESCBY key decrement：key值减少decrement
		INCRBYFLOAT key increment：key值增减increment
		APPEND key value：向key的尾部追加value，若不存在设为value
		STRLEN key：返回key值的长度
		MGET key [key ...]：获取多个键值
		MSET key value [key value ...]：设置多个键值
		（位操作）
		GETBIT key offset：偏移量offset位的值
		SETBIT key offset value：将偏移量offset位上的值设为value
		BITCOUNT key [start end]：获取字符串类型键中值是1的个数，可以指定统计的字节范围
		BITOP operation destkey key [key ...]：对多个字符串类型键进行位运算，并将结果存储在destkey参数指定的键中。
			opertion可以为AND，OR，XOR，NOT。
----------------------------------------------------------------------------------------------------------
	散列：存储列字段(field)和值的映射。
		HSET key field value：将key的field字段设置为value
		HGET key field：取值
		HMSET key field value [field value ...]：多重设值
		HMGET key field [field ...]：多重取值
		HGETALL key：获取key的全部键值
		HEXISTS key field：判断字段是否存在，存在返回1,否则0
		HSETNX key field value：字段不存在时设值
		HINCRBY key field value：递增字段（散列没有HINCR）
		HDEL key field [field ]：删除一个或多个字段，返回删除字段的个数
		HKEYS key：获取字段名
		HVALS key：获取字段值
		HLEN key：获取字段数量
----------------------------------------------------------------------------------------------------------
	列表：有序的字符串列表，可以向列表两端添加删除元素，或获得列表的某一片段，元素有序，非唯一
		LPUSH key value [value ...]：向列表的左侧添加元素
		RPUSH key value [value ...]：向列表的右侧添加元素
		LPOP key：从左端弹出元素
		RPOP key：从右端弹出元素
		LLEN key：列表长度
		LRANGE key start stop：获取索引从start到stop之间的所有元素(包括两端，第一个左侧元素索引为0, 最后一个为-1)
		LREM key count value：删除列表中前count个值为value的元素，返回值为实际删除元素的个数。
			0)count > 0时，从列表左边开始删除前count个值为value的元素
			1)count < 0时，从列表右边开始删除前count个值为value的元素
			2)count = 0时，删除列表中所有值为value的元素
		LINDEX key index：获取索引位置为index上的值
		LSET key index value：将索引位置为index的值设为value
		LTRIM key start end：删除指定索引范围之外的所有元素
		LINSERT key BEFORE|AFTER pivot value：在列表的中从左到右查找pivot，然后在其前|后插入值value
			(若pivot不存在则不插入)
		RPOPLPUSH source destination：从source右侧弹出一个元素放到destination的左边
----------------------------------------------------------------------------------------------------------
	集合：元素无序，唯一，使用散列表实现
		SADD key member [member ...]：增加元素，返回实际增加的元素个数
		SREM key member [member ...]：删除元素，返回成功删除的元素个数
		SMEMBERS key：获取集合中的所有元素
		SISMEMBER key member：判断member是否是集合的元素，返回1或0
		SDIFF key [key ...]：求集合差集
		SINTER key [key ...]：求集合交集
		SUNION key [key ...]：求集合并集
		SCARD key：获取集合中元素个数
		SDIFFSTORE destination key [key ...]：求集合差集并放到destination中
		SINTERSTORE destination key [key ...]：求集合交集并放到destination中
		SUNIONSTORE destination key [key ...]：求集合并集并放到destination中
		SRANDMEMBER key [count]：若无count，从集合中随机取一个元素；若有count：
			0) 当count为正数时，随机从集合中获取count个不重复的元素，count大于元素个数时，返回所有元素
			1) 当count为负数时，随机从集合中获取|count|个元素，可以重复
		SPOP key：从集合中随机删除一个元素
----------------------------------------------------------------------------------------------------------
	有序集合：元素有序，唯一，使用散列表和跳跃表实现，读取中间部分数据时间复杂度O(logN)
		ZADD key score member [score member ...]：增加元素，score可为整数、浮点数及科学计数法表示的数（+inf，-inf）
		ZSCORE key member：获取元素分数
		ZRANGE key start stop [withscores]：按元素分数从小到大返回索引从start到stop之间的元素
		ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]：从小到大返回分数在min和max之间的元素
		ZINCRBY key increment member：增加集合key中元素member的分数，返回增加后该元素的分数
		ZCARD key：获取集合key的元素数
		ZCOUNT key min max：获取指定分数范围内的元素个数
		ZREM key member [member ...]：删除一个或多个元素，返回成功删除元素的个数
		ZREMRANGEBYRANK key start stop：删除排名范围内的元素，返回实际删除元素的个数
		ZREMRANGEBYSCORE key min max：删除指定分数范围内的所有元素，返回实际删除元素的个数
		ZRANK key member：按照元素分数从小到大的顺序获取指定元素的排名
		ZREVRANK key member：按照元素分数从大到小的顺序获取指定元素的排名(分数最大的元素排名为0)
		ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]：
			将集合交集存到destination中
----------------------------------------------------------------------------------------------------------
第四章 进阶
	事务：要么都执行，要么都不执行；所有命令的执行结果会在EXEC后返回
		使用事务：
			MULTI   //事务开始
			[command ...]
			EXEC	//事务提交
		错误处理：
			0) 语法错误：提交的事务中有语法错误(如命令不存在或参数个数不对)时，所有的命令都不会被执行
			1) 运行错误：某个命令出现运行错误(如散列类型的命令作用于集合命令)时，其他正确的命令依旧会执行
	WATCH命令：监控一个或多个键，一旦其中有键被修改或删除，之后的事务就不会再执行；执行EXEC后会取消对所有键的监控，
		也可以使用UNWATCH命令来取消
	EXPIRE：为键设置生存时间，单位是秒
		EXPIRE key time：设置键的生存时间为time秒
		TTL key：查看键离被删除剩余时间(单位为秒)，-1为永久存在，-2为键不存在
		PERSIST key：取消键的生存时间设置（恢复成永久存在）；使用SET或GETSET命令赋值同时也会清除生存时间
		PEXPIRE key time：设置键的生存时间为time毫秒
		PTTL key：查看键离被删除剩余时间(单位为毫秒)
	排序：
		SORT key [ALPHA] [DESC] [LIMIT start count]：key可以为列表、集合或有序集合，若不指定ALPHA，SORT命令会尝
			试将所有元素转换双精度浮点数，无法转换则提示错误；LIMIT限制显示排序后的从start开始的count个元素
		BY参数：‘BY 参考键’，参考键可以是字符串类型或散列类型键的某个字段(表示为键名->字段名)；若提供了BY参数，
			SORT命令不再依据元素的自身的值进行排序，而是对每个元素使用元素的值替换参考键中的第一个“*”并获取其
			值，然后依据该值对元素排序。
			0) SORT tag:ruby:posts BY post:*->time	//*需要在->之前
			1) LPUSH mylist 2 1 3 4
			   SET item:1 50
			   SET item:2 100
			   SET item:3 -10
			   SORT mylist BY item:* DESC
			   RESULT: 2 1 4 3
			当某个参考键的值不存在时，或默认参考键值为0,故4(参考键为0)排在3(参考键为-10)前
			BY后若为常量键名，Redis不会执行排序
		GET参数：不影响排序，作用是使SORT命令返回结果不再是元素自身的值，而是GET参数中指定的键值，参数规则同BY参数
			SORT tag:ruby:posts BY post:*->time DESC GET post:*->title GET post:*->time  //可以有多个GET参数
		STORE参数：将结果存到列表中
			SORT tag:ruby:posts BY post:*->time DESC GET post:*->title GET post:*->time STORE sort.result
		说明：SORT的时间复杂段为O(n + mlogm)，n为要排序列表中元素个数，m为要返回元素个数
			0) 尽可能减小待排序键中元素个数
			1) 使用LIMIT参数只获取需要的数据
			2) 若要排序的数据数量大，尽可能使用STORE参数缓存结果
	消息通知：
		
		
		
		
		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
