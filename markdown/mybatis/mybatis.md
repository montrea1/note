## Mybatis

Mybatis是针对数据持久层的框架，连接数据库。

### xml配置
- configuration（配置）
  - properties（属性）
  - settings（设置）
  - typeAliases（类型别名）
  - typeHandlers（类型处理器）
  - objectFactory（对象工厂）
  - plugins（插件）
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
- databaseIdProvider（数据库厂商标识）
- mappers（映射器）

#### 属性（properties）

配置方式：
``` xml
<!-- xml 设置属性值 -->
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```
``` xml
<!-- xml动态设置属性值 -->
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```
``` java
/* 方法参数传递属性值 */
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, props);

// ... 或者 ...

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, props);
```
如果属性在不只一个地方进行了配置，那么 MyBatis 将按照下面的顺序来加载：

- 在 properties 元素体内指定的属性首先被读取。
- 然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
- 最后读取作为方法参数传递的属性，并覆盖已读取的同名属性。

因此，通过**方法参数传递**的属性具有**最高优先级**，**resource/url** 属性中指定的配置文件**次之**，**最低优先级**的是 **properties** 属性中指定的属性。

#### 设置（setting）
会改变 MyBatis 的运行时行为
```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```
```xml
<settings>
  <!-- 全局地开启或关闭配置文件中的所有映射器已经配置 的任何缓存。 -->
  <setting name="cacheEnabled" value="true"/>
  <!-- 	延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 
        特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。 -->
  <setting name="lazyLoadingEnabled" value="false"/>
  <!--是否允许单一语句返回多结果集（需要驱动支持）。 -->
  <setting name="multipleResultSetsEnabled" value="true"/>
  <!-- 使用列标签代替列名。不同的驱动在这方面会有不同的表现，
       具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 -->
  <setting name="useColumnLabel" value="true"/>
  <!-- 允许 JDBC 支持自动生成主键，需要驱动支持。 
       如果设置为 true 则这个设置强制使用自动生成主键，尽管一些驱动不能支持但仍可正常工作（比如 Derby）。	 -->
  <setting name="useGeneratedKeys" value="false"/>
  <!-- 指定 MyBatis 应如何自动映射列到字段或属性。
         NONE 表示取消自动映射；
         PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。 
         FULL 会自动映射任意复杂的结果集（无论是否嵌套）。 -->
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <!-- 指定发现自动映射目标未知列（或者未知属性类型）的行为。NONE/WARNING/FALLING -->
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <!-- 配置默认的执行器。
        SIMPLE 就是普通的执行器；
        REUSE 执行器会重用预处理语句（prepared statements）； 
        BATCH 执行器将重用语句并执行批量更新。 -->
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <!-- 设置超时时间，它决定驱动等待数据库响应的秒数。 -->
  <setting name="defaultStatementTimeout" value="25"/>
  <!-- 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 -->
  <setting name="defaultFetchSize" value="100"/>
  <!-- 允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为 false。 -->
  <setting name="safeRowBoundsEnabled" value="false"/>
  <!-- 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 -->
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <!-- MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。 
       默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 
       若设置值为 STATEMENT，本地会话仅用在语句执行上，
       对相同 SqlSession 的不同调用将不会共享数据。 -->
  <setting name="localCacheScope" value="SESSION"/>
  <!-- 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。
       某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。 -->
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <!-- 指定哪个对象的方法触发一次延迟加载 -->
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

#### 类型别名（typeAliases）
类型别名是为 Java 类型设置一个短的名字。 它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余。

**使用方式**
```xml
<!-- 类 -->
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

```xml
<!-- 包 -->
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```
```java
/* 注解 */
@Alias("author")
public class Author {
    ...
}
```
**基本类型别名**
别名	| 映射的类型
-|-
_byte |	byte
_long |	long
_short |	short
_int |	int
_integer |	int
_double |	double
_float	 |float
_boolean |	boolean
string |	String
byte |	Byte
long |	Long
short |	Short
int	| Integer
integer |	Integer
double |	Double
float |	Float
boolean |	Boolean
date |	Date
decimal |	BigDecimal
bigdecima | 	BigDecimal
object |	Object
map |	Map
hashmap |	HashMap
list |	List
arraylist |	ArrayList
collection |	Collection
iterator |	Iterator


#### 类型处理器（typeHandlers）

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型

``` java
/* 重写处理器类型 */
// ExampleTypeHandler.java
@MappedJdbcTypes(JdbcType.VARCHAR)
public class ExampleTypeHandler extends BaseTypeHandler<String> {

  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) throws SQLException {
    ps.setString(i, parameter);
  }

  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
  }
}
```

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
```

#### 对象工厂（objectFactory）
&emsp;MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂(ObjectFactory)实例来完成
&emsp;默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认**构造方法**，要么在**参数映射存在**的时候通过**参数构造方法**来实例化。 

```java
// ExampleObjectFactory.java
public class ExampleObjectFactory extends DefaultObjectFactory {
  public Object create(Class type) {
    return super.create(type);
  }
  public Object create(Class type, List<Class> constructorArgTypes, List<Object> constructorArgs) {
    return super.create(type, constructorArgTypes, constructorArgs);
  }
  public void setProperties(Properties properties) {
    super.setProperties(properties);
  }
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }}
```
```xml
<!-- mybatis-config.xml -->
<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/>
</objectFactory>
```
