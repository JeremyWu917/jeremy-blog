# Oracle 数据库基础教程 - To_char 详解

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 数据库中 to_char() 函数是将数值型或日期型转换成字符型，这在前端的数据格式处理中非常常用，因为前端在调用后端 api 接口时，数据格式要严格按照接口的要求来处理，特别是在日期类型的处理上，本节将带领大家讲解一下 to_char 函数的常见用法
</section>


### 转换日期

```sql
-- 语法
to_char(date,[format])
-- 例子
select to_char(sysdate, 'yyyy-MM-dd') from dual;
select to_char(sysdate, 'yyyy-MM-dd hh24:mi:ss') from dual;
-- 返回
2022-01-29
2022-01-29 22:39:00
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. sysdate 为 oracle 数据库自带的关键字，表示当前系统时间
    <br>
    2. 日期格式 yyyy-MM-dd 表示短日期，yyyy-MM-dd hh24:mi:ss 表示 24 小时进制的长日期 
    <br>
    3. 我们也可以使用 to_date('2022-01-29', 'yyyy-MM-dd') 函数将字符格式转换成日期格式
</section>


<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    1. format 格式不可以随意输入
    <br>
    2. to_date 函数第一个参数要输入日期，否则会抛异常
</section>


<br>

### 转换数字

```sql
-- 语法
trunc(num, [format])
-- 例子
select to_char(007) from dual;
select to_char(007, '09999999') from dual;
-- 返回值
7
00000007
```

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    看到这里是不是一头雾水，特别是第二个例子，这里主要涉及到占位符或者称为掩码，但这种应用场景并不多见，接下来，我们用表格说明一下，大家了解即可
</section>

<br>

| 元素 | 说明                         | format    | 例子   | 输出     |
| ---- | ---------------------------- | --------- | ------ | -------- |
| 9    | 数字宽度                     | 999       | 10     | 10       |
| 0    | 显示前面的 0                 | 099       | 10     | 010      |
| .    | 小数点的位置                 | 09.99     | 8.8    | 08.80    |
| D    | 小数分割点的位置             | 099D99    | 8.8    | 008.90   |
| ,    | 逗号的位置                   | 09,99     | 888    | 08,88    |
| G    | 组分隔符的位置               | 09G99     | 888    | 08,88    |
| $    | 美元符号                     | $099      | 8      | $008     |
| MI   | 表示复数的减号的位置         | 99MI      | -888   | 888-     |
| EEEE | 科学计数法                   | 99.99EEEE | 88.888 | 8.89E+01 |
| V    | 乘以10n次（n是V之后9的数量） | 999V99    | 88     | 8800     |

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 以上列出的并非全部，Oracle 版本 11g
    <br>
    2. 了解即可
</section>

### 进制转换

```sql
-- 十进制转成十六进制 
select to_char(888,'XX') from dual;
-- 十六进制转成十进制
select to_number('7D','XX') from dual;
-- 返回
378
125
```



感谢阅读 :coffee:

