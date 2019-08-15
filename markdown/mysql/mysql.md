#mysql高级

## 1.mysql框架

## 2.索引优化
#### sql性能差原因
- 查询sql差
- 索引失效
  - 单值
  - 复合
- 关联join过多
- 服务器调优
 #### 常见通用join查询
 - sql执行顺序
   - 手写
     - ![avatar](/img/sql_writer.png)
   - 机读
     - ![avatar](/img/sql_read.png)
   - 总结
    - ![avatar](/img/sql_parse.png)
 - join图
    - inner join on
       ![avatar](/img/inner_join_on.png)
       ``` sql
        select <select_list> 
        from tableA a
        inner join 
            tableB b
        on 
            a.key=b.key
        ```
    - left join on
      ![avatar](/img/left_join_on.png)
      ``` sql
        select <select_list> 
        from tableA a
        left join 
            tableB b
        on 
            a.key=b.key
         ```
    - right join on
      ![avatar](/img/right_join_on.png)
      ``` sql
        select <select_list> 
        from tableA a
        right join 
            tableB b
        on 
            a.key=b.key
         ```
    - left join on null
       ![avatar](/img/left_join_on_null.png)
       
       ``` sql
        select <select_list>
        from tableA a
        left join on 
            tableB b
        on 
            a.key=b.key
        where 
            b.key is null
       ```
    - right join on null
     ![avatar](/img/right_join_on_null.png)

     ``` sql
      select <select_list>
      from tableA a
      right join on 
          tableB b
      on 
          a.key=b.key
      where 
          b.key is null
     ```
    - full outer join   mysql不支持full outer
    ![avatar](/img/full_outer_join.png)
    ```sql
        selcet <select_list>
        from tableA a
        full outer join
            tableB b
        on
            a.key=b.key

        //mysql union
        select  * from emp e 
        left join 
            dept d 
        on
            e.deptId=d.id 
        union
        select  * from emp e 
        right join 
            dept d 
        on
            e.deptId=d.id  ;
    ```
    - full outer join null
    ![avatar](/img/full_outer_join_null.png)
    ```sql
        selecet <selecet_list>
        from tableA a
        full outer join
            tableB b
        on
            a.key=b.key
        where 
            b.key is null

        //mysql union
        select  * from emp e 
        left join 
            dept d 
        on
            e.deptId=d.id 
        where 
            d.id is null
        union
        select  * from emp e 
        right join 
            dept d 
        on
            e.deptId=d.id 
        where 
            e.deptId is null;
    ```
 - 建表sql

#### 索引
**定义：**（索引数据结构）  
&emsp;索引是帮助mysql高效获取数据的的数据结构

**作用：** 
&emsp;排序和查找。索引会影响where后面查找的和order by排序

**优点：**
&emsp;1.提高数据检索效率，减低io成本  
&emsp;2.通过索引对数据进行排序，降低数据排序成本，降低cpu的消耗

**缺点：**
&emsp;1.索引本身也很大，不可能全部在内存中，因此索引往往以索引文件的形式存在磁盘中    
&emsp;2.降低更新表的速度。更新表示mysql还要保存每次更新添加了索引列的字段 
&emsp;3.索引需要不断变更，成本变高

**Tip：**
&emsp;通常所的索引，如果没有特别说明都是指B树。其中聚集索引、次要索引、覆盖索引、复合索引、前缀索引、唯一索引，默认使用B+树索引，统称索引。（hashIndex）哈希索引

##### 索引分类
单值索引
&emsp;一个索引包含单个列，一个表可以有多个单列索引
唯一索引
&emsp;索引列的值必须唯一，但允许为空值
复合索引    
&emsp;一个索引包含多个列

#### 基本语法
**创建**  
```sql
    cretate [unique] index indexName on tableName(columnname(length));

    alter mytable add [unique] index [indexName] on (columnname(length))
```
**删除**
```sql
    drop index [indexName] on mytable
```
**查看**  
```sql
    show index  from table_name\G
```
**使用alter命令**
![avatar](/img/index_create.png)


#### mysql索引结构
BTree
![avatar](/img/btree.png)
Hash索引
full-text全文索引
R-Tree索引

#### 需要创建索引的情况
1.主键自建唯一索引
2.频繁作为查询条件的字段应该创建索引
3.查询中，与其它表关联的字段，外键关系建立索引
4.频繁更新的字段不适合创建索引  
5.where条件里用不到的字段不创建索引
6.单键/组合索引选择问题（高并发下适合组合索引）
7.查询中排序的字段，可通过索引提高排序速度
8.查询中统计或分组字段

#### 不需要创建索引的情况
1.表记录太少
2.经常增删的表
3.数据重复分布平均的字段

