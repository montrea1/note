## Volatile

#### 应用

解决 **多线程读写存在不一致问题**

#### 解决读写不一致问题

-   总加锁
-   MESI
    -   读操作：不做任何事情，把**Cache中的数据读到寄存器**
    -   写操作：发出信号通知其他的CPU将**改变量的Cache line置为无效**，其他的CPU要访问这个变量的时候，只能从内存中获取。
    -    Cache line  CPU的cache中会增加很多的Cacheline
    -   ​

#### Volatile分析

-   作用
    -   让其他线程能够感知某一线程的某个变量的修改
    -   保证可见性
        -   对共享变量的修改,其它线程马上能感知到
        -   不能保证原子性
    -   保证有序性
        -   重排序（编译阶段、执行阶段
        -   使用Volatile修饰的变量
            -   Volatile前后代码不可越过Volatile调整顺序



#### Volatile与Synchronized区别

-   **作用区域**
    -   Volatile只能修饰变量
    -   Synchronized只能修饰方法和语句块


-   **原子性**
    -   Synchronized能保证原子性
    -   Volatile不能
-   **可见性**
    -   都可以保证可见性
    -   Synchronized
        -   monitorEnter和monitorExit
    -   Volatile
        -   对变量加锁 lock
-   **有序性**
    -   都能保证有序性
    -   Synchronized代价太大。并发退化到串行
-   其他
    -   Synchronized会引发阻塞
    -   Volatile不会

