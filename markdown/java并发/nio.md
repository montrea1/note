## NIO

#### 概念

-   非阻塞io
-   面向缓冲区.数据读取缓冲区，在缓冲区中可以前后移动.
-   nio一个线程能处理多个操作。
-   **数据**总是**从通道被读取到缓冲中**或者**从缓存写入通道中**

#### 核心

**1.Channel通道**

-   **通道**可以**同时读写**,**流**只能**读或者写**
-   通道能实现一步读写数据
-   数据能**读写**到**缓冲**中

**2.Buffer缓冲**

-   可以写读写的数据内存块

-   读写步骤

    -   ```txt
        1.写数据到缓冲区
        2.调用buffer.flip()方法
        3.调用缓冲区读数据
        4.调用buffer.clear()或buffer.compat()方法

        写入时通过flip()记录写下数据,
        读取时buffer从写模式转到读模式
        读模式能读取写入的所有信息
        读读完所有数据后,需要清空缓冲区,让它能再次被写入
        ```

-   Buffer与Channel交互

    -   **Capacity:** buffer容量大小
    -   **Position:**当前读/写的位置
    -   写数据到buffer:position表示当前**待写入位置**,position最大可表示为capacity-1
    -   从缓冲读取数据:position表示从当前**位置读取**

-   **limit:**信息末尾的位置

    -   写模式：表示写入缓冲最大数据
    -   读模式：表示剩余可读取数据
    -   ![avatar](img/readAndwriter.png)

-   **缓冲区常用操作**

    -   写入数据
        -   Channel→Buffer
        -   Buffe.put()
    -   读取数据
        -   Buffer→Channel
        -   Buffer.get()
    -   flip
        -   将Buffer从写模式切换为读模式
        -   position值重置为0,limit改为之前的position
    -   clear
        -   清空缓冲区
    -   compact
        -   清空**已读取**的数据,**未读取**数据保留在buffer

**3.Selector**

-   组件
    -   能够监测多个NIO Channel
    -   检查读写时间是否就绪
    -   Channel以事件的方式注册到Selector
-   ​

![avatar](img/framework.png)



