# Oracle 数据库基础教程 - 函数

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    学完 Oracle 存储过程之后，今天我们再来学习一下 Oracle 函数
    <br>
    两者非常相似，Oracle 函数与存储过程的区别你可以简单的理解为：存储过程和函数都可以返回值，但是函数必须要返回值，并一般只返回一个值，而存储过程可以返回多个值
</section>


### 创建函数

```sql
CREATE OR REPLACE FUNCTION FuncName(v_Str varchar2, v_SubStr varchar2, v_Lot  varchar2 default 'N/A') return varchar2 is
   V_Result   varchar2(50);
   v_Cnt      Integer;
   user_err1 exception; --用户定义异常
   ERRSTR 　　varchar2(200);
begin
   V_Result := 'OK';
   if (条件) then
   --业务逻辑
   --卡控
   --跳到Exit
   GoTo Exit;
   end if;

   if (条件) then
   --业务逻辑
   --卡控
   --跳到Exit
   GoTo Exit;
   end if;

   <<Exit>>
   Return(V_Result);
EXCEPTION
   WHEN user_err1 THEN
      raise_application_error(-20007, ERRSTR);
      RAISE;
end;
```

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. 函数是实现一系列的数据检查，如果不满足条件就直接跳到 Exit 退出函数
    <br>
    2. 函数的最后是要把 V_Result 返回
</section>


### 补充知识

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    接下来我们介绍一下 Oracle 数据库中内置的常用函数
</section>
<br>

| 函数                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `nvl(expression1, expression2)`                              | 从两个表达式中返回一个非 `null` 值                           |
| `decode(field_name, value1, new_value1, value2, new_value2, default_value)` | 针对某个字段，如果它的值为 `value1`,则转换为 `newValue1`，如果值为 `value2`，则转换为 `newValue2`，其他情况显示默认值 |
| `row_number(order by field_name)`                            | 将数据集按照某个字段排序，并产生序号字段。类似的还有 `rowid` 关键字 |
| `to_date(source_string, formater_string)`                    | 将字符串转换为日期类型                                       |
| `to_char()`                                                  | 将其他类型转换为字符串类型，这里面关于日期的转换我们后面单独讲解 |
| `wm_concat`                                                  | 行转列，将多行查询结果聚合到一行的某一列中（不常用）         |
| `listagg() within group(order by field_name) over(partition by field_name)` | 行转列，将多行查询结果聚合到一行的某一列中（不常用）         |
| `concat(expression1, expression2)`                           | 字符串拼接函数，其实我们可以使用 `||` 来拼接                 |
| `sys_guid()`                                                 | 产生并返回一个全球唯一的标识符(原始值)由 `16` 个字节组成，`32` 个字符 |
| `over(partition by field_name, order by field_name)`         | `over` 函数是一个分析函数，和聚合函数搭配在一起使用可以简洁代码 |
| `nlssort`                                                    | 提供简体中文的特殊排序                                       |
| `trunc`                                                      | 是截取日期或数字，根据规则返回指定的值，这里面关于日期的转换我们后面单独讲解 |
| `rank() over(partition by field_name order by field_name)`   | 让返回结果根据分区和排序字段产生排名关系                     |
| `substr(source, start [,length])`                            | 截取字符串                                                   |
| `replace(field_name, sub_str, replace_str)`                  | 将指定的字符串替换为指定的字符串                             |
| `trim`                                                       | 去掉左右两端的空白字符                                       |
| `sign`                                                       | 取数字 `n` 的符号，大于 `0` 返回 `1`， 小于 `0` 返回 `-1`， 等于 `0` 返回 `0` |
| `round(number[,decimal])`                                    | 对数字 `n` 进行四舍五入处理，保留 `decimal` 位小数           |
| `coalesce(expression1,expression2...)`                       | 返回表达式中第一个不为空的值，如果全为空则返回空值           |

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. Oracle 数据库中关于空值或者 NULL 相关的问题上，有时候会产生想不到的异常，这和数据库的版本有关系
	<br>
    2. 一定不要在生产环境中直接修改函数，特别是业务逻辑比较复杂的函数
</section>

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 当函数更新后，与之相关联的对象（过程）都会失效，我们需要更新所有 <strong>失效对象</strong>
    <br>
    2. 切记养成习惯，当我们对对象的数据结构修改后，一定要及时编译失效对象
    <br>
    3. 谨慎修改数据库对象的数据结构，特别是在生产环境中
</section>


感谢阅读 :coffee:

