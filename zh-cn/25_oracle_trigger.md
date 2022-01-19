# Oracle 数据库基础教程 - 触发器

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    触发器是在事件发生时隐式地自动运行的 PL/SQL 程序块，不能接收参数，不能被调用
    <br>
    每当一个特定的数据操作语句（insert\update\delete）在指定的表上发出时，Oracle 自动执行触发器中定义的语句序列
</section>

### 触发器的常见应用场景

- 实时性、重复性的安全应检查
- 数据完整性确认
- 数据的备份与同步

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br><br>
    因为触发器不能接收参数，不能别调用，所以一般情况下特别复杂的业务逻辑里我们要慎用触发器
</section>

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>温馨提醒</strong>
    <br><br>
    对于备份时触发器和快照区别：触发器是同步备份，快照是异步备份
</section>

### 触发器的类型

#### 语句级触发器

这里主要是针对的表，在指定的操作语句**操作之前**或者**操作之后**执行一次，不管这条语句影响了多少行。

#### 行级触发器

这里主要针对的是行，触发语句作用的每一条记录都被触发。在行级触发器中我们使用 `:old` 和 `:new` 来识别值的前后状态。

### 触发器标准语法

#### 数据插入之前执行

```sql
CREATE OR REPLACE TRIGGER triggerName
before insert ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 数据插入之后执行

```sql
CREATE OR REPLACE TRIGGER triggerName
after insert ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 数据更新之前执行

```sql
CREATE OR REPLACE TRIGGER triggerName
before update ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 数据更新之后执行

```sql
CREATE OR REPLACE TRIGGER triggerName
after update ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 数据删除之前执行

```sql
CREATE OR REPLACE TRIGGER triggerName
before delete ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 数据删除之后执行

```sql
CREATE OR REPLACE TRIGGER triggerName
after delete ON tableName for each row
begin
 --业务逻辑
end triggerName;
```

#### 混合情况下的自动触发

```sql
CREATE OR REPLACE TRIGGER triggerName
before INSERT OR UPDATE ON tableName
FOR EACH ROW
DECLARE
--参数
BEGIN
  --业务逻辑
  if :new.texture = '铜丝' then
    :new.texture := 'Cu';
  end if;
  if :new.texture = '金丝' then
    :new.texture := 'Au';
  end if;
END triggerName;
```

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br><br>
    1. triggerName - 触发器名称，唯一不能重复‘
    <br>
    2. 业务逻辑 - 这里写一些触发器出发之后要执行的 SQL 语句
    <br>
    3. 触发器不能接收参数，但是触发器可以定义参数
    <br>
    4. 在业务逻辑处理过程中，我们可以使用 :old 或者 :new 来跟踪表中某个字段的前后变化，从而实现特定的业务判断
</section>

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    基于从业经验告诉大家，慎用触发器！可用可不用的情况下尽量不要使用触发器，特别不要滥用触发器，否则后期维护真的是噩梦。
    <br>
    1. 触发器来实现功能比较隐蔽，不利于团队维护，误删除影响巨大
    <br>
    2. 基于行的触发器会影响性能，特别是对表写操作表多的情况下
</section>

