# Oracle 数据库基础教程 - 存储过程

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    存储过程一般可以理解为：事先经过编译并存储在数据库中的一套 sql 语句
    <br>
    说的更加直白一点：存储过程就是 sql 语句的增删改查操作或者数据检查抛异常的集合
    <br><br>
    优点：1. 提高 sql 执行效率 2. 减少网络流量（I/O）3. 提高系统的安全性
    <br>
    缺点：增加数据库服务器的负荷
</section>

### 创建存储过程

```sql
create or replace procedure p_UserInfo_Delete(v_comment in varchar2, msg out varchar2) as
user_err1 exception;
errstr varchar2(256);
t_cnt number;
begin
 msg := 'Opps';
 select count(*) into t_cnt from UserInfo t where t.Comment = v_comment;
 if t_cnt > 0 then
 	update UserInfo t set t.Comment = 'Updated by admin' where t.Comment = v_comment;
 	msg := 'OK';
 else
 	errstr := 'Opps,data not found!';
 	msg := 'Opps';
 	raise user_err1;
 end if;
exception
  when user_err1 then
  raise_application_error(-20007, errstr);
  raise;
end;
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. 此存储过程实现的功能是将 UserInfo 表中备注信息为 <strong>v_comment(用户输入的条件)</strong> 的结果集把备注信息更新为 <strong>Updated by admin</strong> ，成功时输出 OK，失败或异常时输出 Opps
    <br>
    2. 参数分为 in 和 out 两种，in 关键字表示输入参数，可以省略关键字，out 关键字表示输出参数，不能省略关键字
    <br>
    3. exception 为异常关键字，用户可以通过 raise 关键字进行触发
</section>

### 补充知识

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    对于存储过程和接下来我们要讲解的函数编程来说，我们除了需要掌握一些基础的语法之外还需要做一些常用的知识补充
</section>

#### 变量赋值

```sql
-- 变量声明
vname varchar2(32);
-- 赋值
select 'Jeremy WU' into vname from dual;
vname := 'Jeremy WU';
```

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. 我们可以通过 select 语句进行变量的赋值
    <br>
    2. 或者通过赋值符号 <strong>:=</strong> 对变量进行赋值
</section>

#### 条件判断

```sql
-- 变量声明
vcondition varchar2(32);
-- 变量赋值
vcondition := 'OK';

-- if 条件判断
if vcondition == 'OK' then
  -- 业务逻辑1
else
  -- 业务逻辑2
end if;

-- case when 条件判断
case 
  when vcondition == 'OK' then
    -- 业务逻辑 1
  when vcondition == 'A' then
  	-- 业务逻辑 2
  when vcondition == 'B' then
  	-- 业务逻辑 3
  when vcondition == 'C' then
  	-- 业务逻辑 4
  else -- 业务逻辑 5
end;
  
```

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. 这里和基本的条件分支语法基本类似
    <br>
    2. 注意这种条件判断的语法块开始和结束
    <br>
    3. 对于需要处理的业务逻辑不止一条语句时，需要用 begin end 包裹起来
</section>

#### 游标与循环

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    游标是通过关键字 CURSOR 的来定义一组 Oracle 查询出来的数据集，类似数组一样，把查询的数据集存储在内存当中，然后通过游标指向其中一条记录，通过循环游标达到循环数据集的目的
    <br>
    Oracle 游标可以分为显式游标和隐式游标两种之分，我们这里主要讲解显示游标
    <br>
    Oracle 游标一般与循环配合使用
</section>

```sql
-- 语法
-- 声明游标
declare cursor cursor_name
is select sql_statement;
-- 打开游标
open cursor_name;
-- 取一条游标中的数据
fetch cursor_name into vrecord
-- 关闭游标
close cursor_name;
```

```sql
-- 实例
-- Created on 2019-08-30 by JEREMYWU
declare
  -- Local variables here
  i integer;
  -- 定义游标
  CURSOR Q IS
   SELECT T.Params1, T.Params2, T.Params3 FROM TableName/ViewName T
    WHERE 条件;
begin
  -- Test statements here
  -- 使用游标
  FOR Rec IN Q LOOP
    -- 业务逻辑
    -- 可以使用游标中的字段 Rec.Params1, Rec.Params2, Rec.Params3
  END LOOP;
  -- 或者
  Open Q；
  loop
  	fetch Q into Rec;
  	exit where Q%NOTFOUND;
  	-- 业务逻辑
  	-- 可以使用游标中的字段 Rec.Params1, Rec.Params2, Rec.Params3
  end loop;
  -- 关闭游标
  CLOSE Q；
end;
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 对于第一种方法，我们这里使用了 for ... in ... loop ... end loop 循环
    <br>
    2. 第二种方法我们使用了游标的属性 %NOTFOUND 表示游标获取数据的时候是否有数据提取出来，没有数据返回 true，有数据返回 false
    <br>
    3. 除了 %NOTFOUND 还有 %FOUND、%ISOPEN、%ROWCOUNT，具体意义看名字就知道了
    <br>
    4. 使用完游标后记得关闭游标
</section>

```sql
while 条件语句 loop 
begin 
  -- 业务逻辑
end; 
end loop;
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    除了 for 循环还有 while 循环，这里就不再展开，实际工作过程中根据需要使用
</section>

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 尽量不要使用循环，特别是循环套循环的情况
    <br>
    2. 慎用游标
    <br>
    3. 以上两条都是基于性能考量，后面我们会在 Oracle 数据库优化章节展开讲解，本节不再展开
</section>

### 自治事务

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    首先要先明白会话与事务的含义，事务其实就是数据库的一次 transaction，你也可以简单的理解成一次增加、删除或更新操作。会话是一次 session 可以是一个或者多个事务
    <br>
    自治事务是在某个会话中独立开启一个事务，在其中处理的操作不会影响到同一会话中其他事务未提交的内容
    <br>
    事务具有 4 个属性：原子性、一致性、隔离性和持久性
</section>

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. 在 Oracle 数据库中，有时候我们会希望记录一个过程或者函数的运行日志，不管正常运行结束还是触发异常结束，都要记录
	<br>
    2. 正常结束的没有问题，但是触发异常的情况下，一般的过程或者函数显然不能在插入运行日志之后直接 Commit，因为触发异常后相关业务逻辑需要 RollBack
    <br>
    3. 自治事务就能够很好的避免了这样的问题，就是说自治事务是在某个会话中独立开启一个事务，在其中处理的操作不会影响到同一会话中其他事务未提交的内容
</section>

```sql
--Run_Logs; //运行日志表，包含栏位dates, logs

-- 自治事务存储过程
CREATE OR REPLACE PROCEDURE Pro_Run_Logs(Error_Info In Varchar2)
 Is PRAGMA AUTONOMOUS_TRANSACTION;
BEGIN
　Insert Into Run_Logs(Dates, Logs) Values (Sysdate, Error_Info);
　COMMIT;
END;

-- 一般业务逻辑存储过程
CREATE OR REPLACE PROCEDURE Pro_Test(v_oldcustname in varchar2,v_newcustname in varchar2) is
i number;
errorException exception;
str_err varchar2(100);
user_err exception;
begin
-- 业务逻辑
Commit;
exception
    When errorException Then
         Pro_Run_Logs(str_err);
    WHEN user_err THEN
        raise_application_error(-20007, str_err);
        RAISE;
end;
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 慎用自治事务，特别是在复杂的业务场景中
    <br>
    2. Oracle 主事务严格遵从事务的 4 个属性，一旦开启自治事务，在调用和被调用过程中，如果忘记 commit，往往会造成资源被占用而导致数据库死锁
</section>

感谢阅读 :coffee:

