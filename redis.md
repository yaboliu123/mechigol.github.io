## Redis

1. Redis有哪些数据结构
    - String Hash List set SortSet
    - Bitmap HyperLogLog
2. 速度快的原因
    1. 纯内存
    2. C语言
    3. 单线程
    4. IO多路复用
3. Keys pattern & Scan cursor [pattern] [count]
    - keys指令会导致线程阻塞一段时间，线上服务会停顿,不能线上服务
    - Scan渐进式 O(1)，如果键修改，导致新键没有遍历到，遍历出老键