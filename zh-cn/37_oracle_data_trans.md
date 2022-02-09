# Oracle 数据库基础教程 - 常用知识

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Oracle 数据库的基础教程基本讲解完成，本章作为基础补充主要讲解一些工作中频繁使用的知识
</section>

### 数字处理

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    用 Oracle SQL 语句对数字进行操作: 取上取整、向下取整、保留N位小数、四舍五入、数字格式化
</section>

```sql
-- 取整（向下取整）：
select floor(5.534) from dual;
select trunc(5.534) from dual;
-- 上面两种用法都可以对数字5.534向下取整，结果为5.

-- 如果要向上取整 ，得到结果为6，则应该用ceil
select ceil(5.534) from dual;

-- 四舍五入：
SELECT round(5.534) FROM dual;
SELECT round(5.534,0) FROM dual;
SELECT round(5.534,1) FROM dual;
SELECT round(5.534,2) FROM dual;
-- 结果分别为 6,  6,  5.5,  5.53

-- 保留 N 位小数（不四舍五入）按位数截取：
select trunc(5.534,0) from dual;
select trunc(5.534,1) from dual;
select trunc(5.534,2) from dual;
-- 结果分别是 5,5.5,5.53，其中保留0位小数就相当于直接取整了

-- 数字格式化：
select to_char(12345.123,'99999999.9999') from dual;
-- 结果为12345.123
select to_char(12345.123,'99999999.9900') from dual;
-- 小数后第三第四为不足补0，结果为12345.1230
select to_char(0.123,'99999999.9900') from dual;
select to_char(0.123,'99999990.9900') from dual;
-- 结果分别为 .123, 0.123
```

### 字符串处理

#### 分割

```sql
create or replace function Get_StrArrayLength
(
  av_str varchar2,  --要分割的字符串
  av_split varchar2  --分隔符号
)
return number
is
  lv_str varchar2(1000);
  lv_length number;
begin
  lv_str:=ltrim(rtrim(av_str));
  lv_length:=0;
  while instr(lv_str,av_split)<>0 loop
     lv_length:=lv_length+1;
     lv_str:=substr(lv_str,instr(lv_str,av_split)+length(av_split),length(lv_str));
  end loop;
  lv_length:=lv_length+1;
  return lv_length;
end Get_StrArrayLength;
```

#### 提取

```sql
create or replace function Get_StrArrayStrOfIndex
(
  av_str varchar2,  --要分割的字符串
  av_split varchar2,  --分隔符号
  av_index number --取第几个元素
)
return varchar2
is
  lv_str varchar2(1024);
  lv_strOfIndex varchar2(1024);
  lv_length number;
begin
  lv_str:=ltrim(rtrim(av_str));
  lv_str:=concat(lv_str,av_split);
  lv_length:=av_index;
  if lv_length=0 then
      lv_strOfIndex:=substr(lv_str,1,instr(lv_str,av_split)-length(av_split));
  else
      lv_length:=av_index+1;
     lv_strOfIndex:=substr(lv_str,instr(lv_str,av_split,1,av_index)+length(av_split),instr(lv_str,av_split,1,lv_length)-instr(lv_str,av_split,1,av_index)-length(av_split));
  end if;
  return  lv_strOfIndex;
end Get_StrArrayStrOfIndex;
```

#### 将“目标字符串”以“指定字符串”进行拆分，并通过表结构返回结果

```sql
CREATE OR REPLACE TYPE str_split IS TABLE OF VARCHAR2 (4000);
CREATE OR REPLACE FUNCTION splitstr(p_string IN VARCHAR2, p_delimiter IN VARCHAR2)
    RETURN str_split 
    PIPELINED
AS
    v_length   NUMBER := LENGTH(p_string);
    v_start    NUMBER := 1;
    v_index    NUMBER;
BEGIN
    WHILE(v_start <= v_length)
    LOOP
        v_index := INSTR(p_string, p_delimiter, v_start);

        IF v_index = 0
        THEN
            PIPE ROW(SUBSTR(p_string, v_start));
            v_start := v_length + 1;
        ELSE
            PIPE ROW(SUBSTR(p_string, v_start, v_index - v_start));
            v_start := v_index + 1;
        END IF;
    END LOOP;

    RETURN;
END splitstr;
```

### 获取周别

```sql
CREATE OR REPLACE FUNCTION GET_WEEK (V_RQ in DATE) return varchar2 as
  str  varchar2(20);
  str1 varchar2(20);

begin
  str :=TRIM(TO_CHAR(TRUNC((V_RQ+TO_CHAR(TRUNC(V_RQ,'YYYY'),'D')-1 -TRUNC(V_RQ,'YYYY'))/7)+1,'00'));
  return str;
end;
```

### 快速检索

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    有时候在数据库 Debug 过程中，需要快速查找某个关键字，我们可以通过 Oracle 提供的系统函数快速检索 
</section>

<br>

```sql
SELECT * FROM DBA_SOURCE T WHERE UPPER(T.TEXT) LIKE '%关键字%';
SELECT * FROM DBA_JOBS T WHERE UPPER(T.WHAT) LIKE '%关键字%';
```

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 多积累养成良好的记笔记的习惯
    <br>
    2. 多实践，不同版本的 Oracle 可能会有差异
    <br>
    3. 保持学习
</section>


<br>


感谢阅读 :coffee:

