## JVM
1. Java对象创建  
    - 类加载检查 -> 分配内存 -> 初始化零值 -> 设置对象头 -> init方法
    - 加载 -> 连接 (验证-准备-解析) -> 初始化
    
    - 内存分配  
        1. 指针碰撞 - 内存规整到两边，明显分界线     
        2. 空闲列表 - 内存不规整
    - 对象头: GC分代信息，类元信息，哈希码

2. 对象访问定位  
    1. 句柄
    2. 直接访问
3. Minor GC vs Full GC  
    - Minor GC: 新生代  频繁 速度快  
    - Full GC: 老年代  至少一次MinorGC 速度慢
4. SoftReference & WeakReference
    - SoftReference 内存不足GC  -> 内存敏感的高速缓存
        - 加速JVM对内存回收，防止OOM
    - WeakReference 只要被GC扫描到
    - 两者都和ReferenceQueue配合
        - 对象被gc掉的时候通知用户线程，进行额外的处理时，就需要使用引用队列了。ReferenceQueue即这样的一个对象，当一个obj被gc掉之后，其相应的包装类，即ref对象会被放入queue中。我们可以从queue中获取到相应的对象信息，同时进行额外的处理。比如反向操作，数据清理
5. 无用的类
    1. 无对象实例
    2. 对应的ClassLoader被回收
    3. Class对象不被引用
6. 垃圾回收算法
    1. 标记清除 - 内存碎片
    2. 复制   - 内存分两块 利用率低
    3. 标记整理
    4. 分代
7. 回收器
    - 年轻代 复制  老年代 标记整理
        1. Serial
        2. ParNew
        3. Parallel Scavenge  关注点:吞吐率  -高效率利用CPU
    - 标记清除
        - CMS - 最短回收停顿时间
            初始标记 - 并发标记 - 重新标记 - 并发清除
        - 缺点: 内存碎片 CPU资源敏感  浮动垃圾
    - G1 - 多核 大内存
        - 利用多核并发
        - 整理标记整理  局部复制算法
        - 可预测的停顿时间
        - 优先队列  优先选择回收价值最大的Region
8. 双亲委派
    - BootstrapClassLoader %JAVA_HOME%/lib
    - ExtensionClassLoader %JRE_HOME%/lib/ext
    - AppClassLoader classpath
    - 避免重复加载 
    - 相同的类: 类名  相同的加载器
    - 反例： 自己写java.lang.Object, 如果被自己的Object加载，则不同类
