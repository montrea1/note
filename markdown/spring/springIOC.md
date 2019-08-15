## Spring Ioc
#### 未使用spring
```java
public class Person {
    private String name;
    private String msg;

    public Person() {
    }

    public Person(String msg) {
        this.msg = msg;
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
      Person person=new Person();
        person.setMsg("mengli");
        System.out.println("hello:"+person.getMsg());
    }
}
```

#### 使用spring
1.sprig注册bean。
**applicationContext.xml**
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置bean -->
    <!-- class:类路径 -->
    <bean id="Person" class="com.spring.before.beans.Person">
        <property name="name" value="zhangsan"></property>
        <property name="msg" value="mengli"></property>
    </bean>
</beans>
```
2.实现
``` java
public class Test {
    public static void main(String[] args) {
        ApplicationContext ac=new
                ClassPathXmlApplicationContext("applicationContext.xml");

        Person person =(Person) ac.getBean("Person");
        System.out.println("hello:"+person.getMsg());

    }
}
```
#### 比较
```java
public class Test {
    public static void main(String[] args) {
        //未使用spring
        //new一个bean,对bean设置属性值
        Person person=new Person();
        person.setMsg("mengli");
        System.out.println("hello:"+person.getMsg());

        //使用spring
        //1.在xml中对bean属性设置值
        //2.通过xml配置文件获取配置的bean.
        //3.直接获取bean属性
        ApplicationContext ac=new
                ClassPathXmlApplicationContext("applicationContext.xml");
        Person p =(Person) ac.getBean("Person");
        System.out.println("hello:"+p.getMsg());
    }
}
```
**总结：**
- new 对象
  -  当我们需要一个对象时,不需要再手动创建实例。
  -  通过spring配置，我们可以直接获取到自己想要的对象的实例。

#### IOC 控制反转
**传统方式(正转)**：当我们需要一个实例,我们需要手动实现它。
**IOC方式(反转)**：当我们需要一个实例,通过容器实现该实例.我们直接接收得到该实例

**正转是指程序来创建对象，反转是指程序不创建而是接收对象**






    
