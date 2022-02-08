# Oracle 数据库基础教程 - 杀掉进程

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    kill session 是 DBA 经常碰到的事情之一。如果 kill 掉了不该 kill 的 session，则具有破坏性，因此尽可能的避免这样的错误发生
    <br>
    同时也应当注意，如果 kill 的 session 属于 oracle 后台进程，则容易导致数据库实例宕机
    <br>
    通常情况下，并不需要从操作系统级别杀掉 Oracle 会话进程，但并非总是如此，一般我们会在 Oracle 数据库产生死锁时会在 Oracle级别杀掉会话以及操作系统级别杀掉进程
</section>


### 获取 kill session 信息

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们一般先通过 select 语句查询出非后台进程
</section>

<br>

```sql
SELECT s.inst_id,
         s.sid,
         s.serial#,
         p.spid,
         s.username,
         s.program,
         s.paddr,
         s.STATUS
  FROM   gv$session s
  JOIN gv$process p ON p.addr = s.paddr AND p.inst_id = s.inst_id
  WHERE  s.type != 'BACKGROUND';
```

### 杀掉会话

```sql
ALTER SYSTEM KILL SESSION 'SID1, SID2' IMMEDIATE;
ALTER SYSTEM KILL SESSION 'SID1, SID2';
```

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. SID1, SID2 表示查询到的 SESSION ID，一次杀掉多个进程时，ID 之间需要用逗号隔开
    <br>
    2. IMMEDIATE 官方的解释为立即指示 Oracle 数据库回滚正在进行的事务，释放所有会话锁定，恢复整个会话状态，并立即将控制权归还给您
    <br>
    3. 对于 RAC 环境下的 kill session,需要搞清楚需要 kill 的 session 位于哪个节点，可以查询通过GV$SESSION视图获得。kill session 的时候仅仅是将会话杀掉
    <br>
    4. 在有些时候，由于较大的事务或需要运行较长的 SQL 语句将导致需要 kill 的 session 并不能立即杀掉，对于这种情况将收到 "marked for kill" 提示，一旦会话当前事务或操作完成，该会话被立即杀掉
</section>

### 多会话 kill session 的应用场景

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们一般先通过 select 语句查询出非后台进程的 sid 后，再使用查询生成 kill session 语句，最后执行 kill session 语句即可
</section>

<br>

```sql
select 'alter system kill session '''|| sid ||',' ||SERIAL# ||''''||';'  from gv$session
where sid in ('2731','2734','2720')
order by inst_id;
```

```sql
alter system kill session '2678,8265';   
alter system kill session '2685,83';   
alter system kill session '2720,5';    
alter system kill session '2731,3';    
alter system kill session '2734,15';
```

### 操作系统级别杀掉会话

```sql
SELECT SPID FROM v$process WHERE ADDR IN ('4C61DFF8','4C624730');
KILL SESSION -9 'SPID';
```

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 先通过 ADDR 寻找会话对应的操作系统的 SPID（System Process ID）
    <br>
    2. 再使用 KILL SESSION -9 SPID 命令杀掉操作系统的进程 ID
    <br>
    3. 谨慎操作 -9 命令，很可能会产生意想不到的后果，感兴趣的朋友可以了解一下 -15 与 -9 的区别
</section>

<br>


感谢阅读 :coffee:

