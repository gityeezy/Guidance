# 更多mysql基础

↩️[返回mysql]

## 函数

-- 字符串函数

备注：lpad左侧补齐，rpad右侧补齐，trim去除头尾空格符

```sh
select concat('Hello', 'MySQL');
select lower('Hello');
select upper('Hello');
select lpad('01',5,'-');
select rpad('01',5,'-');
select trim(' Hello MySQL ');
select substring('Hello MySQL', 1, 5);
```

案例：由于业务需求变更，企业员工的工号，统一为5位数，目前不足5位数的全部在前面补0。比如：1号员工的工号应该为00001.

```sh
update emp set workno = lpad(workno,5,'0');
update emp set workno = substring(workno,5,1) where id < 10;
update emp set workno = substring(workno,4,2) where id >= 10;
```

-- 数值函数

备注：mod求a/b的余数，rand求0-1的随机数，round将a保留b位的小数（四舍五入）

```sh
select ceil(1.1);
select ceil(1.9);
select mod(7,4);
select rand();
select round(2.425,2);
```

案例：通过数据库的函数，生成一个六位数的随机验证码

```sh
select lpad(round(rand() * 1000000,0),6,'0');
```

-- 日期函数
备注：curdate-->2023-11-16，curtime-->02:00:09，now-->2023-11-16 02:00:09，
YEAR、MONTH、DAY输出单独的年月日
date_add(a,b)，a+b后的日期
datediff(a,b)，a-b的时间差，单位是天数

```sh
select curdate();
select curtime();
select now();
select YEAR(now());
select MONTH(now());
select DAY(now());
select date_add(now(),interval 70 month );
select datediff('2021-12-01','2021-11-01');
```

案例：查询所有员工的入职天数，并根据入职天数倒序排序

```sh
select name,datediff(curdate(),entrydate) as 入职天数 from emp order by 入职天数 desc ;
```

-- 流程函数

```sh
select if(true,'OK','Error');
select if(false,'OK','Error');
select ifnull('OK','Default');
select ifnull('','Default');
select ifnull(null,'Default');
```

案例：查询emp表的员工姓名和工作地址（北京/上海 -----> 一线城市， 其他 -----> 二线城市）

```sh
select name,if(workaddress='北京' or workaddress='上海','一线城市','二线城市') from emp;
select name,if(workaddress in ('北京','上海'),'一线城市','二线城市') from emp;
select name,
       case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end as 工作地址
from emp;
```

案例：统计班级各个学员的成绩，展示的规则如下
>= 85，展示优秀
>= 60，展示及格
否则，展示不及格

Step 1: 创建表格

```sh
create table score(
    id int comment 'ID',
    name varchar(20) comment '姓名',
    math int comment '数学',
    english int comment '英语',
    chinese int comment '语文'
) comment '学员成绩表';
```

Step 2: 写入数据

```sh
insert into score(id, name, math, english, chinese) values (1,'Tom',67,88,95),(2,'Rose',23,66,90),(3,'Jack',56,98,76);
```

Step 3: 使用case查询

```sh
select
    id,
    name,
    case when math>= 85 then '优秀' when math >=60 then '及格' else '不及格' end as 数学,
    case when english>= 85 then '优秀' when english >=60 then '及格' else '不及格' end as 英语,
    case when chinese>= 85 then '优秀' when chinese >=60 then '及格' else '不及格' end as 语文
from score;
```

case使用小结：
简单函数（我的理解是直接为变量名一一对应）：CASE [column_name] WHEN [value1] THEN [result1]... ELSE [default] END
搜索函数（我的理解是分别设置不同的表达式）：CASE WHEN [expr] THEN [result1]... ELSE [default] END

[返回mysql]:../README.md