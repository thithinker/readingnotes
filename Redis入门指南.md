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
