# Oracle 数据库基础教程 - 表

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 表是 Oracle 数据库的核心，是存储数据的逻辑基础。 
    <br>
    Oracle 表是一个二维的数据结构，由列字段和对应列的数据构成一个数据存储的结构。我们也可以简单看成行和列的二维表，列代表着 Oracle 字段（column），行代表着一行数据（即一条数据记录）
</section>

### 字段数据类型

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>注意</strong>
    <br><br>
    在开始介绍表之前，我们需要了解 Oracle 数据库字段有哪些数据类型，我们这里重点介绍常用的数据类型，按照我个人理解的优先级进行介绍
</section><br>

| 数据类型           | 类型说明                                                     |
| ------------------ | ------------------------------------------------------------ |
| `VARCHAR2(length)` | 字符串类型，`length` 是能够存储的**最大**长度，默认不填的时候是 `1`，最大长度为 `4000` |
| `NUMBER(a, b)`     | 数值类型，顾名思义这里可以存储任意数值，`a` 代表数值的最大位数(小数时，也包含小数点)，`b` 代表小数的位数 |
| `DATE`             | 时间类型，包括年、月、日、时、分、秒                         |
| `TIMESRAMP`        | 时间戳类型，不仅能存储年、月、日、时、分、秒还能存储时区( `Oracle` 内置函数 `systimestamp` 返回的就是时间戳类型) |
| `CHAR(length)`     | 字符串类型，`length` 是字符串的**固定**长度，默认不填的时候是 `1`，最大长度为 `2000` |
| `CLOB`             | 大字段类型，存储的是大文本，当字段长度大于 `4000` 时，可以使用这个数据类型 |
| `BLOB`             | 二进制类型，存储的是二进制对象，比如图片、视频、声音等转换过来的二进制对象 |

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    基于从业经验告诉大家，慎用 BLOB 类型！可用可不用的情况下尽量不要使用，一定不要滥用，否则后期维护真的是噩梦
</section>

### 创建数据表

我们可以通过 `create table` 命令来创建数据表，也可以通过 `PL/SQL` 数据库管理工具来创建数据表

#### 创建用户表

```sql
CREATE TABLE UserInfo
(
	Id varchar2(10) not null,
    UserName varchar2(20) not null,
    Gender char(2) not null,
    Brithday DATE not null,
    Comment varchar2(256) default 'Added by admin'
)
tablespace OA
  storage
  (
    initial 64K
    minextents 1
    maxextents unlimited
  );
comment on table UserInfo
  is '用户信息表';
comment on column UserInfo.Id
  is '主键Id';
comment on column UserInfo.UserName
  is '姓名';
comment on column UserInfo.Gender
  is '性别';
comment on column UserInfo.Brithday
  is '出生日期';  
comment on column UserInfo.Comment
  is '备注信息';  
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong> 
    <br><br>
    1. not null 表示字段不能为空
    <br>
    2. default 表示字段为空时默认输入 Added by admin
    <br>
    3. tablespace 表空间为 OA，storage 表示存储参数：区段(extent)一次扩展 64k，最小区段数为 1，最大的区段数不限制
    <br>
    4. comment on table 是给表名进行注释
    <br>
    5. comment on column 是给表字段进行注释
</section>

#### 添加约束

```sql
-- Create/Recreate primary, unique and foreign key constraints 
alter table UserInfo
  add constraint pk_userinfo_id primary key (Id);

-- Create/Recreate check constraints 
alter table UserInfo
  add constraint ch_userinfo_gender
  check (Gender='男' or Gender='女');
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong> 
    <br><br>
    1. 把 Id 当做主键，主键字段的数据必须是唯一性的
    <br>
    2. 给字段性别添加约束，要么是男要么是女
</section>

#### 添加序列和触发器

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>温馨提醒</strong> 
    <br><br>
    通过之前序列和触发器的学习，我们可以给 UserInfo 表的主键 Id 增加触发器，这样在插入数据时就会自动产生一个不会重复的 Id 了
</section>

```sql
-- Create a sequence for userinfo's id
create sequence seq_userinfo_id
minvalue 1
maxvalue 999999999999999999999999999
start with 1
increment by 1
cache 20;
```

```sql
CREATE OR REPLACE TRIGGER tri_userinfo_id
before insert ON UserInfo for each row
begin
 -- 这边我们不再做合法性验证，不再判断是否超过阈值
 select seq_userinfo_id.nextval into :new.Id from dual;
end tri_userinfo_id;
```

细心的同学该注意到了，`UserInfo` 表的主键 `Id` 字段长度是 `10` ，而触发器生成的 `Id` 会不断地变化，这样表的 `Id` 字段看起来比较混乱长短不齐。

没关系，接下来我们借助 Oracle 自带的左补齐 `lpad` 函数进行补齐（类似的函数还有右补齐 `rpad` ），触发器代码如下：

```sql
CREATE OR REPLACE TRIGGER tri_userinfo_id
before insert ON UserInfo for each row
begin
 -- 不足 10 位时，我们用 0 补齐
 select lpad(seq_userinfo_id.nextval, 10, '0') into :new.Id from dual;
end tri_userinfo_id;
```


<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    我们这里不再讲解表的增删改查操作，后面我们会在单独的章节展开讲解
</section>

