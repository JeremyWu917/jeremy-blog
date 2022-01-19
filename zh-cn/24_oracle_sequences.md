# Oracle 数据库基础教程 - 计数器

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    从今天开始我们来一起学习一下数据库的相关知识。虽然关系型数据库有很多种，但是不管是 Sybase、Oracle、SQL Server、Mysql 还是 PostgreSQL 等语法基本类似，我们以 Oracle 为例进行学习，相信其他关系型数据库也能快速上手
    <br>
    数据库作为数据持久化工具，在全栈开发中是必不可少的，所以接下来的一段时间，我将从基本的语法深入浅出，带你逐步掌握 Oracle 数据库的用法
</section>

### Oracle 数据库简介

`Oracle Database`，又名 `Oracle RDBMS`，简称 `Oracle` 数据库。

`Oracle` 数据库系统是美国 `Oracle` 公司（甲骨文）提供的以分布式数据库为核心的一系列软件产品，是目前世界上使用最为广泛的数据库管理系统，具备完整的数据管理功能，真正实现了分布式处理功能。

`Oracle` 数据库最新版本为 `Oracle Database 19c` 。`Oracle` 数据库 `12c` 引入了一个新的多承租方架构，使用该架构可轻松部署和管理数据库云。此外，一些新特性可最大限度地提高资源使用率和灵活性，这些独一无二的技术进步再加上在可用性、安全性和大数据支持方面的增强，使得 `Oracle` 数据库 `12c` 成为私有云和公有云部署的理想平台。

### 计数器

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 计数器又名 sequences，顾名思义，用来计数的，一般表的主键 ID 都会通过计数器搭配触发器实现自动生成
</section>

#### 标准语法

```sql
-- Create sequence
create sequence Sequence_Name
minvalue 1
maxvalue 999999999999999999999999999
start with 1
increment by 1
-- cycle
cache 20;
```

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br><br>
    这里我们只介绍脚本的方式创建计数器，当然您也可以通过 Oracle 可视化管理工具在 UI 上直接创建
</section>

#### 语法讲解

- `create sequence Sequence_Name` - `create sequence` 为关键字，`Sequence_Name` 为计数器名称，不允许重复
- `minvalue 1` - 最小值为 1
- `maxvalue 999999999999999999999999999` - 最大值为 999999999999999999999999999
- `start with 1` - 从 1 开始
- `increment by 1` - 每次增加 1
- `cycle` - 循环使用，到达最大值或者最小值时，重新建立对象 
- `cache 20` - 缓存为 20

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    maxvalue - 这里的整数最大值最多可以有 28 位数字
    <br>
    increment by 1 - 自增 1，这里如果想要自减，只需要写成负数即可，比如 increment by -1，表示自减 1
    <br>
    cycle - 在一般的应用场景中，我们一般不建议使用循环，当循环到达最大值或最小值时，重新建立计数器对象将有可能对业务产生影响。这也很好理解，实际上就是会重复
    <br>
    cache - 当大量语句发生请求，申请序列时，为了避免序列在运用层实现序列而引起的性能瓶颈。Oracle 序列允许将序列提前生成 cache x 个先存入内存，
在发生大量申请序列语句时，可直接到运行最快的内存中去得到序列。但 cache 个数也不能设置太大，因为在数据库重启时，会清空内存信息，预存在内存中的序列会丢失，
当数据库再次启动后，序列从上次内存中最大的序列号 +1 开始存入cache x 个
</section>

