## spring Aop

#### 静态代理
**代理**：
角色 | 描述
-|-
抽象角色  |  一般使用接口或则抽象类来实现
真实角色  |  被代理角色
代理角色  |  代理真实角色，代理真实角色后，一般做一些附属操作
客户  |  使用代理角色进行一些操作

**静态代理实现**
```java
/* 抽象类 */
public interface Message {
    void editMsg();
}
```
```java
/*被代理对象*/
public class Writer implements Message {
    private String msg;

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    @Override
    public void editMsg() {
        System.out.println("this is writer");
        System.out.println("context : "+msg);
    }
}
```

```java
/*代理对象*/
public class Proxy implements Message{
    Writer writer;//被代理对象

    public Proxy() {
    }

    public Proxy(Writer writer) {
        this.writer = writer;
    }

    public Writer getWriter() {
        return writer;
    }

    @Override
    public void editMsg() {
        getMsg();
        writer.editMsg();
        sendMsg();
    }

    void getMsg(){
        System.out.println("===get Message===");
    }
    void sendMsg(){
        System.out.println("===send Message===");
    }
}
```
```java
/* 客户 */
public class Test {
    public static void main(String[] args) {
        Writer writer=new Writer();
        writer.setMsg("hello word !!!");

        //实现静态代理
        Proxy proxy=new Proxy(writer);
        proxy.editMsg();
    }
}
```

#### 动态代理(有接口)
```java
/* 抽象类 */
public interface Message {
    void editMsg();
}
```
```java
/*被代理对象*/
public class Writer implements Message {
    private String msg;

    public Writer() {
    }

    public Writer(String msg) {
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    @Override
    public void editMsg() {
        System.out.println("this is writer");
        System.out.println("context : "+msg);

    }
}
```
```java
/*动态生产代理对象类*/
public class ProxyInvoationHandler implements InvocationHandler {
    private Object target;
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(),this);
    }

    public void setTarget(Object target) {
        this.target = target;
    }

    public ProxyInvoationHandler() {
    }

    public ProxyInvoationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        Object result=method.invoke(target,args);
        return result;
    }

    public void log(String methodName){
        System.out.println("run "+methodName+"()");
    }
}
```
```java
public class Test2 {
    public static void main(String[] args) {
        Message message=new Writer("hello word");

        ProxyInvoationHandler pih=new ProxyInvoationHandler();
        pih.setTarget(message);
        Message proxy= (Message) pih.getProxy();
        proxy.editMsg();
    }
}
```

#### AOP（面向切面编程）
**springAop**
&emsp;封装动态代理，简化开发。本质：动态代理

**aop在spring中的作用：**
&emsp;1.提供声明式服务（声明式事务）
&emsp;2.允许用户自定义切面
&emsp;3.将公共业务（如日志、安全、权限、缓存、事务等）和领域业务结合。当执行领域业务时，把公共业务加进来。实现公共业务的重复利用。

**aop应用场景:**
日记记录、权限验证、效率检查、事务管理


**springAop组成：**
名称 | 描述
-|-
通知/增强 Adivce  |  增强目标对象的方法
目标对象 Target  |  增强逻辑的织入目标类
连接点 Join point  |  需要增强目标对象的方法
切入点 point cut  |  需要增强目标对象的方法集合
织入 weaving  |  增强目标对象
代理对象 Aop proxy  |  被增强后的目标对象
切面 Aspect  |  切点+增强

![avatar](/img/aop_struction.png)