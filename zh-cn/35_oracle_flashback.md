# Oracle 数据库基础教程 - 数据恢复

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    误删除 Oracle 数据库中的数据，在不使用全库备份恢复或归档日志恢复的情况下，如何快速的恢复被误删除的数据呢？
    <br>
    今天我们来讲解一下 Oracle 数据库数据误删除恢复的 2 种常用方法
</section>

### 闪回

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    闪回（flashback）可以查询某个时间点的数据库状态，然后再进行数据恢复，使用场景一般是 delete，逻辑删除导致的误删除，物理删除是不能被恢复
    <br>
    当然也能够进行数据库的整库恢复，一般不建议这么做，避免脏数据
</section>

#### 例子

```sql
-- 获取删除数据的时间点
select * from v$sql where sql_text like '%table_name%';
-- 根据时间点查询出被删除的数据
select * from table_name as of timestamp to_timestamp('删除时间点', 'yyyy-mm-dd hh24:mi:ss') where (删除时的条件);
-- 恢复数据
insert into table_name select * from table_name as of timestamp to_timestamp('删除时间点', 'yyyy-mm-dd hh24:mi:ss') where (删除时的条件);
```

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. 查询 v$sql 获取删除时间戳，可以使用 upper 函数
    <br>
    2. 利用时间戳和删除条件确认被删除的数据 
    <br>
    3. 重新插入被误删除的数据，需要注意主外键约束
</section>

<br>

### 虚拟回收站

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    oracle 数据库在删除表时会将删除信息存放于某虚拟回收站中而非直接清空，在此种状态下数据库标记该表的数据库为可以复写，所以在该块未被重新使用前依然可以恢复数据，使用场景一般是 drop，逻辑删除导致的误删除，物理删除是不能被恢复
</section>

#### 例子

```sql
-- 查询 user_tables 或者虚拟回收站，找到被删除的表
select user_table, dropped from user_tables;
select object_name, original_name, type, droptime from user_recyclebin;
-- 恢复数据
flashback table object_name to before drop;
flashback table object_name to before drop new_table_name;
```

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. object_name 和 original_name 就是回收站存放的表名和原来删除的表名，如果表名没有被重新命名，可以通过第一条进行恢复误删除的数据
    <br>
    2. 如果不知道源表名，或者需要重新命名新的表名存放数据，则可以通过回收站中的 object_name 进行恢复，即通过第二条进行恢复误删除的数据
</section>

<br>

### 拓展

#### 彻底删除数据

如果确定需要删除的数据又不想无谓的占用空间，我们可以使用以下 `3` 种方式：

1. 采用 `truncate` 方式进行截断。（不能进行数据回恢复）
2. 在 `drop` 时加上 `purge` 选项：`drop table table_name purge`
3. 通过删除 `recyclebin` 区域来永久性删除表 , `drop table table_name cascade constraints purge table table_name`

#### 清空虚拟回收站

```sql
-- 删除当前用户回收站
purge recyclebin;
-- 删除全体用户在回收站的数据
purge dba_recyclebin;
```

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 一般由 DBA 操作，普通用户谨慎操作
    <br>
    2. 了解即可
</section>

感谢阅读 :coffee:

