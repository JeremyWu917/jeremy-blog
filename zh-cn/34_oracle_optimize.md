# Oracle 性能优化

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 数据库的基础教程已基本讲解完成，重要的是接下来多用、多查、多思考总结。今天我们主要讲解一下 Oracle 性能优化
</section>

### 优化方向有哪些

1. `RULE`  (规则) 

2. `COST`  (成本) 

3. `CHOOSE`  (选择性) 

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. 设置缺省的优化器，可以通过对 init.ora 文件中 OPTIMIZER_MODE 参数的各种声明，如 RULE，COST，CHOOSE，ALL_ROWS，FIRST_ROWS 。 你当然也在 SQL 句级或是会话( session )级对其进行覆盖
    <br>
    2. 基于成本的优化器( CBO， Cost-Based Optimizer ) ， 你必须经常运行 analyze 命令，以增加数据库中的对象统计信息( object statistics )的准确性 
    <br>
    3. 如果数据库的优化器模式设置为选择性( CHOOSE )，那么实际的优化器模式将和是否运行过 analyze 命令有关。 如果 table 已经被 analyze 过， 优化器模式将自动成为 CBO ， 反之，数据库将采用RULE形式的优化器
    <br>
    4. 在缺省情况下，Oracle 采用 CHOOSE 优化器， 为了避免那些不必要的全表扫描( full table scan ) ， 你必须尽量避免使用 CHOOSE 优化器，而直接采用基于规则或者基于成本的优化器
</section>

### 访问表的方式

>  `Oracle` 采用两种访问表中记录的方式： 全表扫描和通过 `Rowid` 访问

####    全表扫描 

全表扫描就是顺序地访问表中每条记录。ORACLE采用一次读入多个数据块(database block)的方式优化全表扫描 

#### 通过 Rowid 访问表 

我们可以采用基于 `Rowid` 的访问方式提高访问表的效率， `Rowid` 包含了表中记录的物理位置信息。

`Oracle` 采用索引 `Index` 实现了数据和存放数据的物理位置( `Rowid` )之间的联系

通常索引提供了快速访问 `Rowid` 的方法，因此那些基于索引列的查询就可以得到性能上的提高 

### 共享 Sql 语句

为了不重复解析相同的 `Sql` 语句，在第一次解析之后，`Oracle` 将 `Sql` 语句存放在内存中。这块位于系统全局区域 `SGA` ( `system global area` )的共享池( `shared buffer pool` )中的内存可以被所有的数据库用户共享。因此，当你执行一个 `Sql` 语句（有时被称为一个游标）时，如果它和之前的执行过的语句完全相同， `Oracle` 就能很快获得已经被解析的语句以及最好的执行路径。`Oracle` 的这个功能大大地提高了 `Sql` 的执行性能并节省了内存的使用。 
可惜的是 `Oracle` 只对简单的表提供高速缓冲( `cache buffering` )，这个功能并不适用于多表连接查询。 
数据库管理员必须在 `init.ora` 中为这个区域设置合适的参数，当这个内存区域越大，就可以保留更多的语句，当然被共享的可能性也就越大了。 
当你向 `Oracle` 提交一个 `Sql` 语句，`Oracle` 会首先在这块内存中查找相同的语句。这里需要注明的是，`Oracle` 对两者采取的是一种严格匹配，要达成共享，`Sql` 语句必须完全相同(包括空格，换行等)。 
数据库管理员必须在 `init.ora` 中为这个区域设置合适的参数，当这个内存区域越大，就可以保留更多的语句，当然被共享的可能性也就越大了。

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    共享的语句必须满足三个条件：
    <br>
    1. 字符级的比较： 当前被执行的语句和共享池中的语句必须完全相同
    <br>
    2. 两个语句所指的对象必须完全相同
    <br>
    3. 两个 Sql 语句中必须使用相同的名字的绑定变量( bind variables ) 
</section> 

### 选择最有效率的表名顺序

`Oracle` 的解析器按照从右到左的顺序处理 `From` 子句中的表名，因此 `From` 子句中写在最后的表(基础表 `driving table` )将被最先处理。

在 `From` 子句中包含多个表的情况下，你必须选择记录条数最少的表作为基础表。

当 `Oracle` 处理多个表时，会运用排序及合并的方式连接它们。首先，扫描第一个表( `From` 子句中最后的那个表)并对记录进行派序，然后扫描第二个表( `From` 子句中最后第二个表)，最后将所有从第二个表中检索出的记录与第一个表中合适记录进行合并。 
如果有 `3` 个以上的表连接查询， 那就需要选择交叉表( `intersection table` )作为基础表， 交叉表是指那个被其他表所引用的表。 

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    本条优化规则只在基于规则的优化器中有效！
</section>

### Where 子句中的连接顺序

`Oracle` 采用自下而上的顺序解析 `Where` 子句，根据这个原理，表之间的连接必须写在其他 `Where` 条件之前，那些可以过滤掉最大数量记录的条件必须写在 `Where` 子句的末尾。 

### Select 子句中避免使用  *

当你想在 `Select` 子句中列出所有的 `Column` 时，使用动态 `Sql` 列引用 * 是一个方便的方法。

不幸的是，这是一个非常低效的方法。

实际上，`Oracle` 在解析的过程中， 会将 * 依次转换成所有的列名， 这个工作是通过查询数据字典完成的， 这意味着将耗费更多的时间。 

### 减少访问数据库的次数

当执行每条 `Sql` 语句时，`Oracle` 在内部执行了许多工作：解析 `Sql` 语句，估算索引的利用率，绑定变量，读数据块等等。

由此可见，减少访问数据库的次数，就能实际上减少 `Oracle` 的工作量。 

### 使用 Decode 函数来减少处理时间

使用 `Decode` 函数可以避免重复扫描相同记录或重复连接相同的表。 

### 整合简单，无关联的数据库访问

如果你有几个简单的数据库查询语句，你可以把它们整合到一个查询中(即使它们之间没有关系) 

### 删除重复记录

### 用 Truncate 替代 Delete

当删除表中的记录时，在通常情况下， 回滚段( `rollback segments` ) 用来存放可以被恢复的信息。 

如果你没有 `Commit` 事务，`Oracle` 会将数据恢复到删除之前的状态(恢复到执行删除命令之前的状况)。

而当运用 `Truncate` 时， 回滚段不再存放任何可被恢复的信息。当命令运行后，数据不能被恢复。因此很少的资源被调用，执行时间也会很短。 

### 尽量多使用 Commit

只要有可能，在程序中尽量多使用 `Commit`，这样程序的性能得到提高，需求也会因为 `Commit` 所释放的资源而减少 `Commit` 所释放的资源： 

1. 回滚段上用于恢复数据的信息
2. 被程序语句获得的锁
3. `redo log buffer` 中的空间
4. `Oracle` 为管理上述3种资源中的内部花费

### 计算记录条数

和一般的观点相反，`count(*)` 比 `count(1)` 稍快，当然如果可以通过索引检索，对索引列的计数仍旧是最快的。

### 用 Where 子句替换 Having 子句

避免使用 `Having` 子句，`Having` 只会在检索出所有记录之后才对结果集进行过滤。 这个处理需要排序，总计等操作。如果能通过 `Where` 子句限制记录的数目，那就能减少这方面的开销。 

### 减少对表的查询

在含有子查询的 `Sql` 语句中，要特别注意减少对表的查询。

### 通过内部函数提高 Sql 效率

### 使用表的别名( Alias )

当在 `Sql` 语句中连接多个表时， 请使用表的别名并把别名前缀于每个 `Column` 上。这样一来，就可以减少解析的时间并减少那些由 `Column` 歧义引起的语法错误。 

### 用 Exists 替代 In

在许多基于基础表的查询中，为了满足一个条件，往往需要对另一个表进行联接。在这种情况下，使用 `Exists` (或 `Not Exists` )通常将提高查询的效率。 

### 用 Not Exists 替代 Not In

在子查询中，`Not In` 子句将执行一个内部的排序和合并。无论在哪种情况下，`Not In` 都是最低效的 (因为它对子查询中的表执行了一个全表遍历)。为了避免使用 `Not In` ，我们可以把它改写成外连接( `Outer Joins` ) 或 `Not Exists` 。 

### 用表连接替换 Exists

通常来说 ，采用表连接的方式比 `Exists` 更有效率 

## 用 Exists 替换 Distinct

当提交一个包含一对多表信息(比如部门表和雇员表)的查询时，避免在 `Select` 子句中使用 `Distinct` 。 一般可以考虑用 `Exists` 替换 

### 用索引提高效率

索引是表的一个概念部分，用来提高检索数据的效率，`Oracle` 使用了一个复杂的自平衡 `B-tree` 结构。

通常通过索引查询数据比全表扫描要快。 当 `Oracle` 找出执行查询和 `Update` 语句的最佳路径时, `Oracle` 优化器将使用索引。同样在联结多个表时使用索引也可以提高效率.。另一个使用索引的好处是：它提供了主键( `primary key` )的唯一性验证。

那些 `LONG` 或 `LONG RAW` 数据类型，你可以索引几乎所有的列。

通常在大型表中使用索引特别有效。当然你也会发现，在扫描小表时使用索引同样能提高效率。虽然使用索引能得到查询效率的提高，但是我们也必须注意到它的代价。

索引需要空间来存储也需要定期维护，每当有记录在表中增减或索引列被修改时，索引本身也会被修改。这意味着每条记录的 `INSERT` , `DELETE` , `UPDATE` 将为此多付出 `4` , `5` 次的磁盘 `I/O` 。因为索引需要额外的存储空间和处理,那些不必要的索引反而会使查询反应时间变慢。

定期的重构索引是有必要的：

```sql 
ALTER INDEX <INDEXNAME> REBUILD <TABLESPACENAME>
```

### Sql 语句用大写的

因为 `Oracle` 总是先解析 `Sql` 语句，把小写的字母转换成大写的再执行

### 在 Java 代码中尽量少用连接符 ＋ 连接字符串

### 用 >= 替代 >

高效

```sql 
SELECT * FROM EMP WHERE DEPTNO >= 4
```

低效: 

```sql
SELECT * FROM EMP WHERE DEPTNO > 3
```

两者的区别在于, 前者 `DBMS` 将直接跳到第一个 `DEPT` 等于 `4` 的记录而后者将首先定位到 `DEPTNO=3` 的记录并且向前扫描到第一个 `DEPT` 大于 `3` 的记录

### 用 Union 替换 Or (适用于索引列)

通常情况下，用 `Union` 替换 `Where` 子句中的 `Or` 将会起到较好的效果。对索引列使用 `Or` 将造成全表扫描。

注意：以上规则只针对多个索引列有效

如果有 `Column` 没有被索引，查询效率可能会因为你没有选择 `Or` 而降低 

在下面的例子中， `LOC_ID` 和 `REGION` 上都建有索引

高效: 

```sql
SELECT LOC_ID , LOC_DESC , REGION FROM LOCATION WHERE LOC_ID = 10 UNION SELECT LOC_ID , LOC_DESC , REGION FROM LOCATION WHERE REGION = “MELBOURNE” 
```

低效: 

```sql
SELECT LOC_ID , LOC_DESC , REGION FROM LOCATION WHERE LOC_ID = 10 OR REGION = “MELBOURNE” 
```

如果你坚持要用 `OR` ，那就需要返回记录最少的索引列写在最前面 

### 避免在索引列上使用 IS NULL 和 IS NOT NULL

避免在索引中使用任何可以为空的列，`ORACLE` 将无法使用该索引。对于单列索引，如果列包含空值，索引中将不存在此记录。

对于复合索引，如果每个列都为空，索引中同样不存在此记录。如果至少有一个列不为空，则记录存在于索引中。举例:

如果唯一性索引建立在表的 `A` 列和 `B` 列上，并且表中存在一条记录的 `A` , `B` 值为 ( `123` , `null` ) ，`ORACLE` 将不接受下一条具有相同 `A` , `B` 值（`123` , `null` ）的记录(插入)。

然而如果所有的索引列都为空，`ORACLE` 将认为整个键值为空而空不等于空。因此你可以插入 `1000` 条具有相同键值的记录，当然它们都是空，因为空值不存在于索引列中，所以 `WHERE` 子句中对索引列进行空值比较将使`ORACLE` 停用该索引. 
低效: (索引失效) 

```sql
SELECT … FROM DEPARTMENT WHERE DEPT_CODE IS NOT NULL; 
```

高效: (索引有效) 

```sql
SELECT … FROM DEPARTMENT WHERE DEPT_CODE >=0;
```

### 用 Union-all 替换 Union

当 `SQL` 语句需要 `UNION` 两个查询结果集合时，这两个结果集合会以 `UNION-ALL` 的方式被合并，然后在输出最终结果前进行排序。

如果用 `UNION ALL` 替代 `UNION`, 这样排序就不是必要了。效率就会因此得到提高。

需要注意的是，`UNION ALL` 将重复输出两个结果集合中相同记录。因此各位还是要从业务需求分析使用 `UNION ALL` 的可行性。

`UNION` 将对结果集合排序，这个操作会使用到 `SORT_AREA_SIZE` 这块内存。对于这块内存的优化也是相当重要的。

### 需要当心 Where 子句

某些 SELECT 语句中的 WHERE 子句不使用索引，记住：

1. 索引只能告诉你什么存在于表中，而不能告诉你什么不存在于表中

2. `||` 是字符连接函数，就象其他函数那样，停用了索引
3. `+` 是数学函数，就象其他数学函数那样，停用了索引
4. 相同的索引列不能互相比较，这将会启用全表扫描

### 优化 Group By

提高 `Group By` 语句的效率, 可以通过将不需要的记录在 `Group By` 之前过滤掉.下面两个查询返回相同结果但第二个明显就快了许多

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 带有 DISTINCT,UNION,MINUS,INTERSECT,ORDER BY 的 SQL 语句会启动 SQL 引擎执行耗费资源的排序( SORT )功能
    <br>
    2. DISTINCT 需要一次排序操作, 而其他的至少需要执行两次排序
    <br>
    3. 通常, 带有 UNION, MINUS , INTERSECT 的SQL 语句都可以用其他方式重写
    <br>
    4. 如果你的数据库的 SORT_AREA_SIZE 调配得好, 使用 UNION , MINUS, INTERSECT 也是可以考虑的, 毕竟它们的可读性很强
</section>

<br>

感谢阅读 :coffee: