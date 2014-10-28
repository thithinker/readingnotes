第一章 简介
    Redis： 开源的高性能键值对数据库。
    Redis支持的键值数据类型：字符串类型，散列类型，列表类型，集合类型，有序集合类型。
    Redis可以为每个键设置生存时间，到期自动删除，始于作为缓存系统；列表类可以用来实现队列，支持阻塞式读取，易于实现高性能的优先级队列；支持“发布/订阅”的消息模式。

第二章 准备
    安装Redis：1. 下载redis-stable.tar.gz
        2. tar xzf redis-stable.tar.gz
        3. cd redis-stable
        4. make
    启动： redis-server  //默认使用6379端口，可用“redis-server --port 6379” 指定启动端口。
    关闭： redis-cli shutdown   //Redis收到SHUTDOWN命令后，先断开所有客户端连接，然后根据配置执行持久化，最后完成退出。
    Redis命令客户端： redis-cli -h 127.0.0.1 -p 6379   //连接
        redis-cli   //使用默认参数（同上）连接
    	
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
	列表：
		
		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
