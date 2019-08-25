##Synchronized

#### 概念

-   **原理**
    -   利用锁机制来实现同步
-   **锁机制特性**
    -   互斥性（原子性）
        -   在同一时间内只允许一个线程有有个对象锁.
        -   同一时间只有一个线程对同步代码块进行操作
    -   可见性
        -   必须保证锁被释放之前,对共享变量所做的修改,**对于后来获得锁的另一个线程时可见的**
        -   否则另一个线程时在本地缓存上的副本,在副本上操作,引起数据一致性

#### synchronized用法

**1.根据修饰对象分类**

-   **同步类**

    -   1.同步非静态方法

        -   ```java
            public synchronized static void mothodName(){ }		
            ```

    -   2.同步静态方法

        -   ```java
            public synchronized void mothodName(){ }
            ```

-   同步代码块

    -   1.获取对象锁

        -   ```java
            synchronized(this|object)()
            ```

    -   2.获取类锁 

        -   ```java
            synchronized(类.class)
            ```

#### synchronized原理

**加锁**

​	代码块加锁:monitor、monitorexit

​	方法加锁:ACC_SYCHRONIZED

**Monitor**

-   0,lock

-   重入

-   monitor一个线程占有,其它线程请求进入会被BLOCK,知道monitor为0

-   ```java
    1.每一个对象都会有一个monitor对象监视器
    2.某一个线程占有这个对象时,先判断monitor的计数器不是0
    	0：没被线程占有   !0 :已被其它线程占有
    	被线程占用时:monitor+1    线程释放:monitor-1
         同一个线程可以对同一个对象进行多次加锁+1、+1...冲入性
     3.monitor与对象头
     对象头:加锁基础
    ```

**MonitorExit**

-   计数器减一,为0时为解锁
-   两个monitorexit.1个为正常退出,一个为异常退出

**synchronized优化**

-   偏向锁
    -   偏向第一次被占有的线程
    -   CAS
    -   0 01
-   轻量级锁
    -   线程有交替使用互斥性不是很强
    -   00
-   重量级锁
    -   强互斥
    -   10
-   自旋锁
    -   竞争失败时,不马上转化级别,二十执行几次空循环
-   锁消除
    -   JIT在编译时把不必要的锁消除



​	

