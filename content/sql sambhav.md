---
tags:
  - system-design-review
sr-due: 2026-07-07
sr-interval: 1
sr-ease: 230
---

```

```


```==
mysql> SELECT T1.ITEM_NO, T1.ITEM, T2.Sname
> FROM STORE T1
> JOIN SUPPLIERS T2 ON T1.Scode = T2.Scode;

+---------+------------------+--------------------+
| ITEM_NO | ITEM             | Sname              |
+---------+------------------+--------------------+
|    2005 | Sharpner Classic | Soft plastics      |
|    2003 | Ball Pen 0.25    | Tetra Supply       |
|    2002 | Gel Pen Premium  | Premium Stationary |
|    2006 | Gel Pen Classic  | Premium Stationary |
|    2001 | Eraser Small     | Tetra Supply       |
|    2004 | Eraser Big       | Tetra Supply       |
|    2009 | Ball Pen 0.5     | Premium Stationary |
+---------+------------------+--------------------+
7 rows in set (0.00 sec)




```

```



describe store;

+---------+--------------+------+-----+---------+-------+
| field   | type         | null | key | default | extra |
+---------+--------------+------+-----+---------+-------+
| item_no | int(11)      | no   | pri | null    |       |
| item    | varchar(50)  | yes  |     | null    |       |
| scode   | int(11)      | yes  | mul | null    |       |
| qty     | int(11)      | yes  |     | null    |       |
| rate    | decimal(10,2)| yes  |     | null    |       |
| lastbuy | date         | yes  |     | null    |       |
+---------+--------------+------+-----+---------+-------+
6 rows in set (0.01 sec)


```

```
mysql> select t2.sname, avg(t1.rate) as average_rate
> from store t1
> join suppliers t2 on t1.scode = t2.scode
> where t2.sname in ('Premium Stationary', 'Tetra Supply')
> group by t2.sname;

+--------------------+--------------+
| sname              | average_rate |
+--------------------+--------------+
| Premium Stationary |    16.666667 |
| Tetra Supply       |    13.000000 |
+--------------------+--------------+
2 rows in set (0.00 sec)
```


```
mysql> select item, qty, rate
> from store
> order by rate desc;

+-----------------+------+-------+
| item            | qty  | rate  |
+-----------------+------+-------+
| Ball Pen 0.25   |   50 | 25.00 |
| Gel Pen Classic |  250 | 20.00 |
| Ball Pen 0.5    |  180 | 18.00 |
| Gel Pen Premium |  150 | 12.00 |
| Sharpner Classic|   60 |  8.00 |
| Eraser Big      |  110 |  8.00 |
| Eraser Small    |  220 |  6.00 |
+-----------------+------+-------+
7 rows in set (0.00 sec)
```

```
mysql> select s.class, s.sec, s.sname
> from student s
> join st-house h on s.house = h.hid
> where h.hname = 'NARMADA';

+-------+-----+---------+
| class | sec | sname   |
+-------+-----+---------+
|    12 | C   | PALLAVI |
|    12 | D   | KIRAN   |
+-------+-----+---------+
2 rows in set (0.00 sec)
```

```
mysql>  select count(*) as total_students
> from student;

+----------------+
| total_students |
+----------------+
|              4 |
+----------------+
1 row in set (0.00 sec)
```

```
mysql> select sname
> from student
> order by sname desc;

+---------+
| sname   |
+---------+
| SAMPATH |
| ROHAN   |
| PALLAVI |
| KIRAN   |
+---------+
4 rows in set (0.01 sec)
```


```
mysql> delete from student
> where sec = 'A';

Query OK, 2 rows affected (0.01 sec)
```


```
select teacher, periods
from school
where periods > 25;

+-----------+---------+
| teacher   | periods |
+-----------+---------+
| priya rai |      26 |
| lis anand |      27 |
| ganan     |      28 |
| harish b  |      27 |
+-----------+---------+
```

```
select *
from school
order by experience desc;

+------+--------------+-----------+------------+---------+------------+
| code | teacher      | subject   | doj        | periods | experience |
+------+--------------+-----------+------------+---------+------------+
| 1215 | umesh        | physics   | 1998-05-11 |      22 |         16 |
| 1045 | yashraj      | maths     | 2000-08-24 |      24 |         15 |
| 1009 | priya rai    | physics   | 1998-09-03 |      26 |         12 |
| 1001 | ravi shankar | english   | 2000-03-12 |      24 |         10 |
| 1203 | lis anand    | english   | 2000-04-09 |      27 |          5 |
| 1167 | harish b     | chemistry | 1999-10-19 |      27 |          5 |
| 1123 | ganan        | physics   | 1999-07-16 |      28 |          3 |
+------+--------------+-----------+------------+---------+------------+̌
```

```
select distinct designation
from admin;

+------------------+
| designation      |
+------------------+
| vice principal   |
| coordinator      |
| hod              |
| senior teacher   |
+------------------+
```

```
select school.teacher, school.code, admin.designation
from school
join admin
on school.code = admin.code
where admin.gender = 'male';

+--------------+------+------------------+
| teacher      | code | designation      |
+--------------+------+------------------+
| ravi shankar | 1001 | vice principal   |
| yashraj      | 1045 | hod              |
| ganan        | 1123 | senior teacher   |
| harish b     | 1167 | senior teacher   |
| umesh        | 1215 | hod              |
+--------------+------+------------------+
```

```
select *
from consumer
order by consumer_name desc;

+------+---------------+-----------+------+
| c_id | consumer_name | city      | s_id |
+------+---------------+-----------+------+
| 06   | writer well   | mumbai    | gp02 |
| 12   | topper        | delhi     | bp01 |
| 01   | pen house     | delhi     | pl01 |
| 16   | motivation    | bangalore | pl01 |
| 15   | good learner  | delhi     | pl02 |
+------+---------------+-----------+------+
```

```
select stationary_name, price
from stationery
where price between 10 and 15;

+-----------------+-------+
| stationary_name | price |
+-----------------+-------+
| ball pen        |    10 |
| gel pen         |    15 |
+-----------------+-------+
```

```
select c.consumer_name, c.city, s.stationary_name
from consumer c
join stationery s
on c.s_id = s.s_id
where s.company = 'reynolds';

+---------------+---------+-----------------+
| consumer_name | city    | stationary_name |
+---------------+---------+-----------------+
| writer well   | mumbai  | gel pen         |
| topper        | delhi   | ball pen        |
+---------------+---------+-----------------+
```

```
update stationery
set price = price + 2;

Query OK, 5 rows affected
```

```
+------+-----------------+----------+-------+
| s_id | stationary_name | company  | price |
+------+-----------------+----------+-------+
| bp01 | ball pen        | reynolds |    12 |
| pl02 | pencil          | natraj   |     7 |
| er05 | eraser          | natraj   |     5 |
| pl01 | pencil          | apsara   |     8 |
| gp02 | gel pen         | reynolds |    17 |
+------+-----------------+----------+-------+
```

```
select m_company, m_name, m_price
from mobile_master
order by m_mf_date desc;

+-----------+----------+---------+
| m_company | m_name   | m_price |
+-----------+----------+---------+
| sony      | xperiam  |    7500 |
| micromax  | unite3   |    4500 |
| samsung   | galaxy   |    4500 |
| nokia     | n1100    |    2250 |
| oppo      | selfieex |    8500 |
+-----------+----------+---------+
```

```
select *
from mobile_master
where m_name like 's%'
or m_name like '%a';

+-------+-----------+----------+---------+------------+
| m_id  | m_company | m_name   | m_price | m_mf_date  |
+-------+-----------+----------+---------+------------+
| mb001 | samsung   | galaxy   |    4500 | 2013-02-12 |
| mb006 | oppo      | selfieex |    8500 | 2010-08-21 |
+-------+-----------+----------+---------+------------+
```

```
select m_id, sum(m_qty) as total_quantity
from mobile_stock
group by m_id;

+-------+----------------+
| m_id  | total_quantity |
+-------+----------------+
| mb001 |            300 |
| mb003 |            400 |
| mb004 |            450 |
| mb006 |            200 |
+-------+----------------+
```

```
select *
from mobile_master
where m_price > 5000;

+-------+-----------+----------+---------+------------+
| m_id  | m_company | m_name   | m_price | m_mf_date  |
+-------+-----------+----------+---------+------------+
| mb005 | sony      | xperiam  |    7500 | 2017-11-20 |
| mb006 | oppo      | selfieex |    8500 | 2010-08-21 |
+-------+-----------+----------+---------+------------+
```

