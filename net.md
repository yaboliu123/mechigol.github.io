## 网络
1. TCP - 面向连接、可靠  
    - 三次握手  SYN - SYN/ACK - ACK  
    - 原因: 双方确认自己/对方 发送/接受正常
        1. Client 什么都不能确认；Server 确认了对方发送正常，自己接收正常 
        2. Client 确认了：自己发送、接收正常，对方发送、接收正常；Server 确认了：对方发送正常，自己接收正常
        3. Client 确认了：自己发送、接收正常，对方发送、接收正常；Server 确认了：自己发送、接收正常，对方发送、接收正常
    - 为什么还要传回SYN? 
        - 告诉发送端,我接受的接受到的信息确实是你发送的
    - 发了SYN, 还要ACK?
        - SYN说明发送方到接收方的通道没问题，接受方到发送方的通道需要ACK验证
    - 四次握手 FIN - ACK - FIN - ACK
        - 任何一方都可以在数据传送结束后发出连接释放的通知，待对方确认后进入半关闭状态。当另一方也没有数据再发送的时候，则发出连接释放通知，对方确认后就完全关闭了TCP连接。
        
        - 举个例子：A 和 B 打电话，通话即将结束后，A 说“我没啥要说的了”，B回答“我知道了”，但是 B 可能还会有要说的话，A 不能要求 B 跟着自己的节奏结束通话，于是 B 可能又巴拉巴拉说了一通，最后 B 说“我说完了”，A 回答“知道了”，这样通话才算结束

2. TCP 如何保证可靠传输
    1. 应用数据被分割成 TCP 认为最适合发送的数据块。
    2. TCP 给发送的每一个包进行**编号**，接收方对数据包进行排序，把有序数据传送给应用层。
    3. **校验和**： TCP 将保持它首部和数据的检验和。这是一个端到端的检验和，目的是检测数据在传输过程中的任何变化。如果收到段的检验和有差错，TCP 将丢弃这个报文段和不确认收到此报文段。
    4. TCP 的接收端会**丢弃重复**的数据。
    5. **流量控制**： TCP 连接的每一方都有固定大小的缓冲空间，TCP的接收端只允许发送端发送接收端缓冲区能接纳的数据。当接收方来不及处理发送方的数据，能提示发送方降低发送的速率，防止包丢失。TCP 使用的流量控制协议是可变大小的滑动窗口协议。 （TCP 利用**确认包滑动窗口**实现流量控制，从而影响发送方）
    6. **拥塞控制**： 当网络拥塞时，减少数据的发送。
    7. **ARQ协议(自动重传请求)**： 也是为了实现可靠传输的，它的基本原理就是每发完一个分组就停止发送，等待对方确认。在收到确认后再发下一个分组。
    8. **超时重传**： 当 TCP 发出一个段后，它启动一个定时器，等待目的端确认收到这个报文段。如果不能及时收到一个确认，将重发这个报文段。
3. 浏览器中输入url地址 ->> 显示主页的过程
    1. DNS解析
    2. TCP连接
    3. 发送HTTP请求
    4. 服务器处理请求并返回HTTP报文
    5. 浏览器解析渲染页面
    6. 连接结束
4. URI URL 区别
    -   URI(Uniform Resource Identifier) 是统一资源标志符，可以唯一标识一个资源。
    - URL(Uniform Resource Location) 是统一资源定位符，可以提供该资源的路径
5. Https 默认443