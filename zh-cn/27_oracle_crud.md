# Oracle 数据库基础教程 - 表的增删改查

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    上一节我们讲解了 Oracle 数据表的创建，本节我们简单讲解一下数据表的增删改查操作，这也是前端业务逻辑的最终体现，您可以这么认为：前端用户的所有操作体现到后端数据库持久化时都会增删改查之一！
</section>

### 开始之前

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>温馨提醒</strong>
    <br><br>
    我们还是以上一节的用户信息表为例进行增删改查操作，但是我们不再添加各种约束，以便更好的为大家解释说明
</section>

#### 创建用户表

```sql
CREATE TABLE UserInfo
(
	Id varchar2(10) not null,
    UserName varchar2(20) not null,
    Gender char(2) not null,
    Birthday DATE not null,
    Comment varchar2(256) default 'Added by admin'
)
```

### 查询数据

#### 例子

```sql
select * from UserInfo;
select t.UserName, t.Gender, t.Birthday from UserInfo t;
select * from UserInfo t where t.Gender = '男';
select * from UserInfo order by Id desc;
select * from UserInfo t where t.Gender = '男' or t.Comment like 'Added%';
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 第一条语句 * 号表示查询 UserInfo 表的所有字段的数据
    <br>
    2. 第二条语句表示只查出 UserInfo 这张表的 UserName, Gender, Birthday 三个字段的数据，t 是为了书写便利为 UserInfo 起的别名
    <br>
    3. 第三条语句 where 后面跟的是条件，表示查出这张表性别是男生的所有数据
    <br>
    4. 第四条语句 order by filed desc 表示对查询结果按照 filed 降序排序，省略 desc 或者换成 sal 表示的是升序排序
    <br>
    5. or 表示的是或者的关系，只要满足条件之一即可(and 表示并列，必须同时满足)，like 后面中的 % 表示通配符
    <br>
    6. 尽量不要使用 *，出于性能的考虑最好是按需查询需要的字段，谨慎使用 %, 因为这回造成索引的失效，后面我们会在 <strong>性能优化</strong> 章节中详细讲解
    <br>
    7. 尽量使用表别名，特别是多表联查的时候，后面我们会在 <strong>视图</strong> 章节中详细讲解
</section>

### 插入数据

#### 例子

```sql
-- 增加一条数据
insert into tableName (field1, field2, ..., fieldn) values (value1, value2, ..., valuen);
-- 增加多条数据
insert into tableName (field1, field2, ..., fieldn) 
select value1, value2, ..., valuen from dual
union select value1, value2, ..., valuen from dual
union select value1, value2, ..., valuen from dual;
-- 或者
insert into tableName (field1, field2, ..., fieldn) 
select value1, value2, ..., valuen from tableName2;
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. field 表示数据表的字段名称
    <br>
    2. value 的字段类型要和对于的 field 的数据类型一致
    <br>
    3. select * from dual 表示把 * 查出来以数据行的形式返回
    <br>
    4. union（后面我们会讲解 union all） 表示将多个数据行连接在一起
    <br>
    5. 最后一条表示把 tableName2 表中的全部数据插入到 tableName 表中，这里需要注意的是对应字段的数据类型要完全一致
</section>

### 更新数据

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    对于 <strong>更新</strong> 和 <strong>删除</strong> 操作，一定要非常谨慎，否则将造成极端后果，一定 <strong>不要</strong> 在生产环境中做测试，虽然我们可以通过快照的方式恢复数据，但是也尽量避免此类低级错误！
</section>

#### 例子

```sql
update UserInfo t set t.Comment = '2022 虎年大吉！';
update UserInfo t set t.Comment = '123' where t.Comment = 'Added by admin';
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 第一句表示更新 UserInfo 表中所有的行数据 Comment 字段的值为 <strong>2022 虎年大吉！</strong> ,结果将会影响所有行
    <br>
    2. 第二句表示更新 UserInfo 表中 Comment 字段的值为 <strong>Added by admin</strong> 的所有数据行，将 Comment 字段的值更新为 <strong>123</strong>，结果影响有限行
    <br>
    3. 我们不建议不加条件的更新（语句一），尽量避免这样的操作
    <br>
    4. 强烈建议在执行更新操作之前，先使用 <strong>更新条件</strong> 执行查询操作，确认查询结果没问题时再执行更新操作
</section>

### 删除数据

#### 例子

```sql
delete UserInfo;
delete from UserInfo t where t.Comment = '123';
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 第一句表示删除 UserInfo 表中所有的数据,结果将会影响所有行
    <br>
    2. 第二句表示删除 UserInfo 表中 Comment 字段的值为 <strong>123</strong> 的所有数据行，结果将会影响有限行
    <br>
    3. 和更新操作类似我们不建议不加条件的删除（语句一），尽量避免这样的操作
    <br>
    4. 强烈建议在执行删除操作之前，先使用 <strong>删除条件</strong> 执行查询操作，确认查询结果没问题时再执行删除操作
</section>

### 拓展知识

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们可以通过如下语句对数据表进行删除
</section>

```sql
truncate table UserInfo drop storage;
truncate table UserInfo reuse storage;
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 第一句表示删除 UserInfo 表并清空表所占用的空间（ <strong>drop storage</strong> 为关键字可以省略）
    <br>
    2. 第二句表示删除 UserInfo 表不清空表所占用的空间
</section>

