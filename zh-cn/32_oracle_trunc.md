# Oracle 数据库基础教程 - Trunc 详解

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 数据库 Trunc 函数是非常常用的函数，我们可以通过 Trunc 函数对 <strong>数字</strong> 或 <strong>日期</strong> 类型的数据进行按需截取
</section>

### 截图数字

```sql
-- 语法
trunc(number1, [, number2])
-- 例子
select trunc(12.87, 1) from dual;
select trunc(12.87, 0) from dual;
select trunc(12.87) from dual;
select trunc(12.87, -1) from dual;
-- 返回
12.8
12
12
10
```

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. number1 为被截图的数字，number2 为需要保留的小数位，是一个可选项
    <br>
    2. 如果 number2 为负数时，表示截取小数点左边的 number2 位数字，<strong>被截取部分记为 0</strong> 
    <br>
    3. 如果 number2 省略时，表示截图所有的小数位，只保留整数位
</section>

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    Trunc 函数截取数字时，当截取参数为负数时需要特别注意，被截取的部分需要补 0
</section>

<br>

#### 截取日期

```sql
-- 语法
trunc(date, [, format])
-- 例子
select trunc(sysdate, 'YEAR') from dual;
select trunc(sysdate, 'DD') from dual;
select trunc(sysdate) from dual;
-- 返回值
2022/1/26
2022/1/26
2022/1/26
```

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>解释说明</strong>
    <br><br>
    1. date 为输入的日期值，是必输项。format 为指定格式来截取输入的日期值，是一个可选项
    <br>
    2. 当省略 format 时，默认为 DD 格式，返回四舍五入后的日期或截取到这天午夜的时间
</section>

#### format 的格式如下

<br>

| 名称                                                     | 解释说明                                                     |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| `CC` 、`SCC`                                             | 世纪                                                         |
| `SYYYY`、`YYYY` 、`YEAR` 、 `SYEAR` 、`YYY` 、`YY` 、`Y` | 当前年，`y` 表示年的最后一位 `yy` 表示年的最后 `2` 位 `yyy` 表示年的最后 `3` 位 `yyyy` 用 `4` 位数表示年 |
| `IYYY` 、 `IY` 、`IY` 、`I`                              | 当前 `ISO` 格式的年                                          |
| `Q`                                                      | 当前时间的季度                                               |
| `MONTH` 、`MON` 、`MM`、`RM`                             | 当前时间的月，`mm` 用 `2` 位数字表示月；`mon` 用简写形式 比如 `11` 月或者 `nov` |
| `WW`                                                     | 当前时间年的第几周                                           |
| `IW`                                                     | 当前时间 `ISO` 格式的年的第一天                              |
| `W`                                                      | 当前月的第几周                                               |
| `DDD` 、`DD` 、 `J`                                      | 当前时间的天                                                 |
| `DAY` 、`DY` 、`D`                                       | 当前周的第一天                                               |
| `HH`、`HH12` 、`HH24`                                    | 小时、`12` 进制小时、`24` 进制小时                           |
| `MI`                                                     | 分钟                                                         |

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. ISO 格式标准认为一周的开始是从周一到周日
    <br>
    2. IW 格式在年份跨越时周是连着的
    <br>
    3. 在开发过程中我们一般采用 I 标准
</section>

感谢阅读 :coffee:

