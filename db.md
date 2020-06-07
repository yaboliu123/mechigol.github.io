## 数据库
1. InnoDB vs MyISAM
    - 事务
    - 行锁  MyISAM表级锁
    - 崩溃恢复
    - 外键
    - MVCC
2. InnoDB vs MyISAM 数据存放
    - MyISAM data域存放数据地址
    - InnoDB data域存放数据本身  -> 聚簇索引
3. 并发问题
    - 肮读  Read-Uncommited
    - 不可重复读  Read-commited会有问题，只要commit就能读到
    - 幻读 T1读，T2插入，T1再读
    - select @@tx_isolation 
4. Next-Key lock 解决幻读  gap lock 防止在一个范围内插入数据
5. 数据库分片方案
    1. 客户端代理Sharding-JDBC
    2. 中间件 MyCat
6. 分布式id  
    原因: 每个数据库的id都是从1开始  
    1. UUID  缺点:太长 无序
    2. 数据库id   缺点:独立部署，维护成本，性能瓶颈
    3. Redis
    4. Snowflake
7. 数据库查询顺序  
<img src="./image/db_query.png" height="300" width="800"/>
8. 数据库更新  
<img src="./image/db_update.png" height="700" width="800"/>

9. redo log vs. binlog  
    - redo:     
        - 物理日志 引擎层 InnoDB特有   
        - WAL (Write-Ahead Logging) 先日志，后更新内存  
        - 固定大小，比如4个文件每个1G
        - 作用: 数据异常处理恢复  Crash-safe
    - binlog: 
        - 逻辑日志
        - 日志归档 用于备份、主从复制
10. 两阶段提交意义:
    1. **先写redo log后写binlog**。假设在redo log写完，binlog还没有写完的时候，MySQL进程异常重启。由于我们前面说过的，redo log写完之后，系统即使崩溃，仍然能够把数据恢复回来，所以恢复后这一行c的值是1。
    但是由于binlog没写完就crash了，这时候binlog里面就没有记录这个语句。因此，之后备份日志的时候，存起来的binlog里面就没有这条语句。
    然后你会发现，如果需要用这个binlog来恢复临时库的话，由于这个语句的binlog丢失，这个临时库就会少了这一次更新，恢复出来的这一行c的值就是0，与原库的值不同。

    2. **先写binlog后写redo log**。如果在binlog写完之后crash，由于redo log还没写，崩溃恢复以后这个事务无效，所以这一行c的值是0。但是binlog里面已经记录了“把c从0改成1”这个日志。所以，在之后用binlog来恢复的时候就多了一个事务出来，恢复出来的这一行c的值就是1，与原库的值不同。