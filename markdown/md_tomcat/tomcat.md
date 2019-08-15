## Tomcat 

##### 顶层结构
    server
        service
            connector
            container
            jasper
            naming
            session
            loging
            jmx
            .....
    
    简要说明：
        server：
            Tomcat的最顶层，代表整个服务器。
            每个server拥有至少一个service
        service：
            主要部分connector、container
            一个service包含一个或多个connector
            一个service只能有一个container
        connector:
            用于处理连接相关的事情。
            提供Socket与request和response的相关转化
            connector需要实现TCP/IP和http协议
        container：
            用于封装个管理servlet以及request请求
            

#### Tomcat接收处理请求流程
##### Tomcat 接收请求
Tomcat由connector接收请求
connector架构：![avatar](/img/connector.png)

###### ProtocolHandler
    不同的的ProtocolHandler代表不同的连接
    ProtocolHandler
        Endpoint
            Acceptor
            Handler  
            AsyncTimeout
        Processor   
        Adapter

###### Endpoint
    处理socket网络连接。
###### Processor   
    Processor将接受到的Socket封装成request
###### Processor   
    Adapter将request传给container

##### Tomcat处理请求
Tomcat使用container处理请求。
container用于封装和管理servlet以及具体处理request请求
#####  container架构
    Engine
        Host                 虚拟机
            Context          应用
                Wrapper      servlet

###### Engine
    引擎。用来管理多个站点，一个service只能有一个Engine
###### Host
    代表一个站点。每一个host代表一个虚拟机。
    可以通过配置Host添加站点
###### Context
    每一个Context代表一个应用
    对应web.xml。对应开发的一个程序
###### Wrapper
    每一个Wrapper封装一个servlet
    
##### container处理请求
    container采用pipe-valve管道处理请求
    每个管道只负责做自己对应的处理，处理完后，请求返回，让下一管道处理。

    每个Pipeline都有一个特定的Valve，管道最后执行的叫BaseValve。
    BaseValve不可删除，且会调用下一个管道。

    Container包含4个子容器。子容器对应的baseValve
    StandardEngineValve
    StandardHostValve
    StandardContextValve
    StandardWrapperValve

![avatar](/img/container.png)

当执行到StandardWrapperValve的时候，会**在StandardWrapperValve中创建FilterChain**，并调用其doFilter方法来处理请求，这个FilterChain包含着我们配置的与请求相匹配的Filter和Servlet，其**doFilter方法会依次调用所有的Filter的doFilter方法和Servlet的service方法，这样请求就得到了处理！**





