# Oracle 数据库基础教程 - 视图

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    视图也称为虚表，一般来说不占用物理空间（定义视图的 SQL 语句是存储在数据字典里的），是客制化 SQL 语句查询的逻辑定义，是从一个或多个基表或 <strong>视图</strong> 中得到的
    <br>
    我们这里不再讨论数据库操作权限的问题，默认为 DBA 权限
</section>

### 创建视图

#### 简单视图

```sql
create or replace view v_UserInfo as 
select a.Id, a.UserName, a.Email from UserInfo a where a.Comment = '123' order by a.Id desc;
```

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>说明</strong>
    <br><br>
    1. 因为我们这里用了 <strong>replace</strong> 关键字，所以为了防止覆盖之前的视图，执行创建语句之前，我们先看下这个名称的视图是否存在
    <br>
    2. UserInfo 表是用户信息表，表字段包括 Id，UserName，Email，Comment 等
    <br>
    3. 我们将 UserInfo 表中备注信息为 123 的数据集合查出来按照主键 Id 排序后组成了 v_UserInfo 视图
</section>


#### 复杂视图

```sql
create or replace view v_UserInfo_Detail as 
select a.Id, a.UserName, a.Email, b.DeptName from UserInfo a, Department b where a.DeptId = b.Id order by a.Id desc with read only;
```

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>说明</strong>
    <br><br>
    1. 因为我们这里用了 <strong>replace</strong> 关键字，所以为了防止覆盖之前的视图，执行创建语句之前，我们先看下这个名称的视图是否存在
    <br>
    2. UserInfo 表是用户信息表，表字段包括 Id，UserName，Email，DeptId 等
    <br>
    3. Department 表是部门信息表，表字段包括 Id，DeptName 等
    <br>
    4. 用户信息表中存着部门信息表的主键 Id，这样两张表就能够通过 Id 进行关联；
    <br>
    5. <strong>with read only</strong> 关键字表明此视图为只读视图
    <br>
    6. 实际上视图存在的意义，可以说一定程度上简化了用户的 SQL 查询语句的书写。比如在此例中，后续用户只需要查询 v_UserInfo_Detail 这张视图即可获取到想要的信息，不需要再写复杂的 SQL 查询语句
</section>


<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    在 Oracle 数据库中除了这种常见的视图，还有一种视图，叫做 <strong>物化视图</strong> ，有兴趣可以自行了解一下，本节不再展开
</section>

### 使用视图

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    当我们创建好了视图之后，接下来我们就可以愉快的使用视图了，一般情况下，对于简单视图我们可以把它当作数据表来使用
</section>

我们可以根据需要对视图进行增删改查操作，这里我们就不再对操作语法进行展开了，您可以参考前一节 <strong>表的增删改查</strong>，进行学习。

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 视图的增删改查与数据表的增删改查，操作语法上是基本上是没有区别
    <br>
    2. 视图的增删改，不管是简单视图还是复杂视图，在做增删改的时候，基本上都会出现问题，因为数据表一般都会有主外键等约束条件，而视图的做基本组成是数据表
    <br>
    3. 我们不建议对视图做增删改操作，特别是复杂视图
</section>

### 更新视图

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    当有业务需要时，我们常常需要修改视图，比如视图字段的增加或减少，我们只需要编辑后重新执行一遍 create or replace view ... 即可（和创建时一样）
</section>

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 当视图更新后，与之相关联的对象（视图、过程、函数等）都会失效，我们需要更新所有 <strong>失效对象</strong>
    <br>
    2. 同样的当我们修改表的结构时，与之相关联的对象也会失效，我们需要更新所有 <strong>失效对象</strong>
    <br>
    3. 切记养成习惯，当我们对对象的数据结构修改后，一定要及时编译失效对象
    <br>
    4. 谨慎修改数据库对象的数据结构，特别是在生产环境中
</section>

### 删除视图

```sql
drop view viewName;
```

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 当视图删除后，与之相关联的视图都会失效，我们需要更新所有 <strong>失效对象</strong>
    <br>
    2. 同样的视图删除后，与之相关联的 <strong>失效对象</strong> ，一般都需要更新，不是单纯的重新编译一下就可以解决的
    <br>
    3. 切记养成习惯，当我们对对象的数据结构修改后，一定要及时编译失效对象
    <br>
    4. 不建议删除视图
</section>


### 常用的数据字典视图

| 对象名称           | 解释说明                              |
| ------------------ | ------------------------------------- |
| `dba_views`        | `DBA` 视图，数据库中的所有视图        |
| `all_views`        | `ALL` 视图，用户可访问的视图          |
| `user_views`       | `USER` 视图，用户拥有的视图           |
| `dba_tab_columns`  | `DBA` 视图，数据库中的所有视图/表的列 |
| `all_tab_columns`  | `ALL` 视图，用户可访问的视图/表的列   |
| `user_tab_columns` | `USER` 视图，用户拥有的视图/表的列    |

#### 例子

```sql
select a.view_name, a.text from user_views a where upper(a.text) like '% USERINFO %';
```

<br>
<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>说明</strong>
    <br><br>
    1. 这里是查询出用户拥有的视图中包含 UserInfo 关键字的所有视图
    <br>
    2. upper 方法表示把小写转换成大写
    <br>
    3. like 关键字与 % 配合进行模糊匹配查询
    <br>
    4. 查询比较慢
</section>



感谢阅读 :coffee:
