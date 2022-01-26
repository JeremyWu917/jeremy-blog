# Oracle 数据库基础教程 - 计划任务

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    学完 Oracle 函数和存储过程之后，接下来我们可以学习计划任务了
    <br>
    我们可以使用计划任务定期按指定的时间间隔执行某个存储过程或者函数来实现某个业务逻辑的处理
</section>

### 创建计划任务

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. 想要执行计划任务我们需要为 Oracle 数据库安装计划任务服务器组件，这里不再展开可根据需要自行安装
	<br>
    2. 需要注意 Oracle 版本 10g 之前的版本需要引入 DBMS_JOBS 包，之后的版本需要引入 DBMS_SCHEDULER 包
</section>

```sql
-- 10g
declare
  jobNo number;
begin
  dbms_job.submit(job => jobNO,
                    what => 'p_UserInfo;',
                    next_date => sysdate,
                    interval => 'trunc(sysdate + 1) + 1/24'
                 );
  commit;
end;
```

```sql
-- 11g
variable 
  jobNo number;
begin
  sys.dbms_job.submit(job => :jobNo,
                        what => 'p_UserInfo;',
                        next_date => sysdate,
                        interval => 'trunc(sysdate + 1) + 1/24'
                     );
  commit;
end;
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. declare 定义了 pl/sql 中的局部变量，variable 定义了全局变量，第一次会生成唯一的脚本编号
    <br>
    2. what 对应的值就是我们要执行的 Oracle 函数或者存储过程
    <br>
    3. next_date 对应的值表示下次要执行的日期/天
    <br>
    4. interval 对应的值表示时间间隔
    <br>
    5. commit 一定不要忘记加，以免造成数据库死锁，特别是在自治事务场景下
</section>

### 补充知识

#### Oracle 数据库内置的计划任务

<br>

| 计划任务名称       | 解释说明                 |
| ------------------ | ------------------------ |
| `user_jobs`        | 当前用户的计划任务       |
| `dba_jobs_running` | 正在执行的计划任务       |
| `dba_jobs`         | 执行完成的计划任务       |
| `all_jobs`         | 数据库系统所有的计划任务 |

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. dba_jobs 和 all_jobs 其实在使用过程中基本没有差别
	<br>
    2. 我们可以通过 select 语句快速检索出我们想要查询的计划任务，比如 select * from dba_jobs t where upper(t.what) like '%P_USERINFO%'
</section>

#### 常用的时间间隔

| 时间间隔                                    | 解释说明                                |
| ------------------------------------------- | --------------------------------------- |
| `trunc(sysdate)+26/24`                      | 每天凌晨 2 点                           |
| `trunc(next_day(sysdate,1))+26/24`          | 每周凌晨 2 点                           |
| `trunc(last_day(sysdate))+26/24`            | 每月 1 号凌晨 2 点                      |
| `trunc(add_months(sysdate,3),'Q')+2/24`     | 根据第一次执行的时间点，每季度凌晨 2 点 |
| `add_months(trunc(sysdate,'yyyy'),6)+2/24`  | 根据第一次执行的时间点，每半年凌晨 2 点 |
| `add_months(trunc(sysdate,'yyyy'),12)+2/24` | 根据第一次执行的时间点，每年凌晨 2 点   |
| `sysdate + 1`                               | 根据第一次执行的时间点，每天            |
| `sysdate + 1/24`                            | 根据第一次执行的时间点，每小时          |
| `sysdate + 10/（60*24）`                    | 根据第一次执行的时间点，每 10 分钟      |
| `sysdate + 30/(60*24*60)`                   | 根据第一次执行的时间点，每 30 秒        |
| `sysdate + 7`                               | 根据第一次执行的时间点，每周            |
| `sysdate+1/1440`                            | 根据第一次执行的时间点，每分钟          |

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. Oracle 计划任务的时间间隔是在执行完成之后才去计算下一次的执行时间
    <br>
    2. 以上常用的时间间隔除了前 3 条，其他的时间间隔都会在一定程度上导致 <strong>时间漂移</strong> ，因为越复杂的业务逻辑执行所花费的时间越长
    <br>
    3. 考虑到执行计划任务需要时间，所以我们要合理评估执行业务逻辑所需要的时间，然后合理设置时间间隔，不建议太小，以免资源竞争造成死锁
</section>

感谢阅读 :coffee:

