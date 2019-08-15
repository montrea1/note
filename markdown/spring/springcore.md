## SpringIOC
#### 依赖注入
    XML bean definitions, 
    annotated components (that is, classes annotated with @Component, @Controller, and so forth), 
    or @Bean methods in Java-based @Configuration classes. 

**依赖注入流程**
- The **ApplicationContext is created and initialized** with configuration metadata that describes all the beans. Configuration metadata can be specified by XML, Java code, or annotations.
- **For each bean,** its dependencies are expressed in the form of properties, constructor arguments, or arguments to the static-factory method (if you use that instead of a normal constructor). These dependencies are provided to the bean, when the bean is actually created.
- **Each property or constructor argument is an actual definition of the value to set, or a reference to another bean in the container.**
- **Each property or constructor argument** that is a value is converted from its specified format to the actual type of that property or constructor argument. By default, Spring can convert a value supplied in string format to all built-in types, such as int, long, String, boolean, and so forth.

1.初始化配置文件，注册配置元素。（xml、javacode、annotation）
2.遍历所有bean，获取bean的声明元素，这些声明会被用来创建bean
3.遍历bean中的声明元素的值
4.遍历bean声明元素中的值进行转换成对应的数据类型

**注入方式**
1.构造器注入
2.setter方法注入
