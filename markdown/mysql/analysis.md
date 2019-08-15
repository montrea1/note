## 性能分析
#### mysql常见瓶颈
>**cpu**
&emsp;cpu在饱和的时候一般会发生在数据载入内存，或从磁盘上读取数据的时候
**Io**
&emsp;装入数据远大于内存容量的时候    
**服务器硬件性能瓶颈**
&emsp;top,free,iostat和vmstat查看系统性能状态


 #### Explain
 **作用：**
 &emsp;模拟优化器执行sql查询语句，从而知道mysql是如何处理sql语句。分析查询的语句或表结构的性能瓶颈

 **应用：**
 >1.表的读取顺序
 2.数据读取操作的操作类型
 3.那些索引可以使用
 4.那些索引被实际使用
 5.表之间的引用
 6.每张表有多少行被优化器查询

 #### Explain字段
    id
    select_type
        simple
        primairy
        subquery
        derived
        union
        union result
    table
    type
        all
        idnex
        range
        ref
        eq_ref
        const,system
        null
    possible_keys
    key
    key_len
    ref
    rows
    Extra
        using filesort
        using temporary
        using index
        using where
        using join buffer
        impossible where
        select tables optimzed away
        distinct
![avatar](/img/explain_row.png)

 **id**
>&emsp;select查询序列号，包含一组数字，表示查询执行select子句或操作表的顺序  
**三种情况:**
   1.id相同，执行顺序由上至下
   2.id不相同,如果是子查询，id序号会递增，id值越大优先级越高，月先执行
   3.id相同不相同，相同时存在

**select_type**
>&emsp;查询的类型，主要用于区别：
&emsp;普通查询、联合查询、子查询等复杂查询
>**类型：**
1.simple
&emsp; 简单select查询，查询中不包含子查询或union
2.primary
&emsp;查询中若包含任何复杂的子部分，最外层查询则被标记
3.subquery
&emsp;select 或where列表中包含了子查询
4.derived
&emsp;在from列表中包含的子查询被标记为derived（衍生）
&emsp;mysql会递归这些子查询，把结果放在临时表里
5.union
&emsp;若第二个select在union之后出现，则被标记为union
&emsp;若union包含在from子句的子查询中，外层select将被标记为 derived
6.union result
&emsp;从union表获取结果的select

**table**

**type**
&emsp;访问类型排列,显示查询使用了何种类型
```
    从最好到最差：
    system>const>eq_ref>ref>fulltext>ref_or_null>index_merge>unique_subquery>index_subquery>range>index>all
    system>const>eq_ref>ref>range>index>all

    system:
        const类型的的特例。忽略不计
    const:
         通过一次索引就找到。const用于比较primary key或者unique索引。只匹配一行数据
    eq_ref
        唯一索引扫描，对每个索引键，表中只有一个记录与之匹配。常用于主键或唯一索引扫描
    ref
        非唯一性索引扫描。返回匹配某个单独值的所有行
        本质是索引访问，可能会找到多个符合条件行，属于查找和扫描的混合体
    range
        值检索索引范围的行，使用一个索引选择行。key显示使用了哪个索引
        在where语句中出现 between 、<、>、in等范围查询。不需要全部扫描
    index
        全索引扫描。index只遍历索引树。通常比all快。
    all
        全盘扫描
```
**possible_keys**
&emsp;显示可能应用在这张表的索引。一个或多个
&emsp;查询字段上若存在索引，索引将被列出，不一定会被使用
**key**
&emsp;实际使用的索引。如果为null则没使用该索引
&emsp;查询中若使用了覆盖索引，则该索引仅出现在key列表中

**key_len**
&emsp;表示索引中使用的字节数。通过该列计算索引长度。在不损失精度情况下，长度越短越好
&emsp;显示值为索引字段的最大长度，并非实际可用长度。根据表定义计算而得，而非表内检索而得。

**ref**
&emsp;显示索引的那一列被使用了，如果可能的话，是一个常数。哪些列或常量被用于查询索引列上的值

**rows**
&emsp;根据表统计信息及索引选用情况，大致估算出所需记录所需要读取的行数

**Extra**
- **useing filesort**
  - 说明mysql会对数据使用一个外部排序索引。而不是按照表内索引顺序读取。
  - mysql无法利用索引完成的排序操作：**文件排序**
- **using temporary**
  - 使用临时表保存中间结果，mysql对查询结果排序时使用临时表。
  - 常见于排序order by 和 分组查询 group by
- **using index**
  - 表示响应的select操作中使用了覆盖索引，避免访问了数据行。
  - 如果同时出现using where 表明索引被用来执行索引键值的查找
     如果没有出现using where 表明索引用来读取而非执行查询动作
   - **覆盖索引**
     - select数据列值用从索引中就能获取，不必读取数据行。
     - mysql利用索引返回select列表中的字段，不必根据索引再次读取数据文件。  **查询列要被所建的索引覆盖**
     - 如果要使用覆盖索引，一定要注意select列表中只取出需要的列，不可select *
     - 查询所有字段，会导致索引文件过大，查询性能下降
   - **using where**
     - 表明使用了where过滤
   - **using join buffer**
     - 使用了连接缓存
   -  **using impossible**
      -  where 子句的值·总是false，不能用来获取任何元组
   - **using tables optimzed away**
     - 在没有group by子句的情况下，基于索引优化min/max操作或者对myISAM存储引擎优化count(*)操作。不必等到执行阶段再进行计算，查询执行计划生成阶段即将完成优化
   - **distinct**
     - 优化distinct操作，在找到第一项的元组后,立即停止查找同样值的动作


#### case

![avatar](/img/case1.png)

**索引分析**

**单表**
  - 1.检查需要检索的字段

**两表**
  - left join on
    - 在右表创建索引
  - right join on
    - 在左表创建索引

**三表**
  - 三表左连接,两表建立索引
    -  **join语句优化**：
    - 1.小表驱动大表。减少NestedLoop循环总次数
    - 2.优先优化NestedLoop内层循环
    - 3.保证join语句中被驱动表上join条件字段已经被索引
    - 4.当无法保证被驱动表的join条件字段被索引，且内存资源充足的前提下，不要吝啬joinBuffer的设置


**避免索引失效**
  - 1.全值匹配
  - 2.最佳左前法则.
    - 创建索引，不要跳过索引中的列
    - 第一个索引列不能跳过，中间的索引列不能跳过
  - 3.不要在索引列上做任何操作（计算，函数，自动或手动转型）。
    - 出现全表扫描
  - 4.存储引擎蹦使用索引中范围条件右边的列
  - 5.尽量使用覆盖索引（只访问索引的查询，索引列和查询列一致）,减少select *
  - 6.mysql在使用不等于（!=或<>）的时候无法使用索引导致全表扫描
  - 7. is null ，is not null 无法使用索引
  - 8.like 以通配符开头（'%abc'）mysql索引 失效会变成全表扫描
    - like 最好情况%在右边
    - 双边% 建议使用覆盖索引
  - 9.字符串不加单引号索引失效
  - 10.少用or,用它来连接时索引会失效
  - 


**tips:**
1.使用范围可能会导致索引失效

**sql分析：**
>1.慢查询开启并捕获
2.Explain+慢sql分析
3.show profile查询mysql服务器里执行的细节和生命周期情况
4.slq数据库服务器参数调优



### 排序优化
#### 小表驱动大表
 **in exists**
![avatar](/img/rbo.png)

**order by**
  - 尽量使用index，避免filestort
  - order by 默认使用升序排列
  
  - 使用index情况
    - order by语句使用索引最左前列
    - 使用where子句与order by子句条件列组合满足索引最左前列
  - select * 出现问题
    - **参数调优：**
      - sort_buffer_size
      - max_length_for_sort_data
- 排序使用索引 ![avatar](/img/order_by.png)
  
**group by**
  - 先排序后分组，遵循最左前缀
  - 无法使用索引列时，可增大max_length_for_sort_data
  - where 高于having。能在where写，就不在having写

#### 慢查询日志
慢查询日志：
查看： show variables like '%slow_query_log%'
开启： 
set global slow_query_log=1;(当前数据库有效，关闭数据库够无效)
可以修改数据库配置文件my.cnf，mysqld下添加参数永久生效
&emsp;slow_query_log=1;
&emsp;slow_query_log_file=fileValue（该参数的值）
&emsp;long_query_time=4;
&emsp;log_output=FILE

设置阈值：
> show global variables like 'long_query_time%';
set global long_query_time=3;

查询慢查询日志记录
select * sleep (Time) //时间
show global status like '%Slow_queries%;


日志分析工具 mysqldumpslow
>s:表示按照何种方式排序
c:访问次数
l:锁定时间
r:返回记录
t:查询时间
al:平均锁定时间
ar:平均返回时间
at:平均查询时间
t:返回前面多少条的数据
p后边搭配一个正则匹配模式，大小写不敏感

![avatar](/img/slowdump.png)


#### 数据库脚本执行
**1.建表**
**2.设置参数log_bin_trust_function_creators**
![avatar](/img/log_bin.png)

**3.创建函数，保证每条数据都不同**
**4.创建存储存储过程**
**5.调用存储过程**

#### show profile