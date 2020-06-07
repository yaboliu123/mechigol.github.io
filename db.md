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