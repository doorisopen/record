# Join
* INNER 조인
* LEFT OUTER 조인
* RIGHT OUTER 조인
* FULL (OUTER) 조인
* 카티전 조인 (cross 조인)
* 셀프 조인


## SQL에서 Join의 유형 
* __(INNER) JOIN:__ 두 테이블에서 일치하는 값을 가진 레코드를 리턴합니다.
* __LEFT (OUTER) JOIN :__ 왼쪽 테이블의 모든 레코드와 오른쪽 테이블의 일치하는 레코드를 반환합니다
* __RIGHT (OUTER) JOIN :__ 오른쪽 테이블의 모든 레코드와 왼쪽 테이블의 일치하는 레코드를 반환합니다
* __FULL (OUTER) JOIN :__ 왼쪽 또는 오른쪽 테이블에 일치하는 모든 레코드를 반환합니다

![db_sql_join_1](/images/ComputerScience/Database/Sql/db_sql_join_1.JPG)

## Demo DataBase
다음과 같은 테이블이 있다고 해보자.

```
CREATE TABLE animal_A ( 
id VARCHAR(30) NOT NULL PRIMARY KEY, 
name VARCHAR(30) NOT NULL,
type VARCHAR(30) NOT NULL,
reg_date TIMESTAMP 
);

CREATE TABLE animal_B ( 
id VARCHAR(30) NOT NULL PRIMARY KEY,
name VARCHAR(30) NOT NULL,
type VARCHAR(30) NOT NULL,
reg_date TIMESTAMP
); 
-- animal_A
INSERT INTO `animal_A` (`id`, `name`, `type`, `reg_date`) VALUES ('A352713', 'Jewel', 'cat', '2017-08-13 13:50:00');
INSERT INTO `animal_A` (`id`, `name`, `type`, `reg_date`) VALUES ('A352714', 'Cherokee', 'dog', '2017-07-08 09:41:00');
INSERT INTO `animal_A` (`id`, `name`, `type`, `reg_date`) VALUES ('A352715', 'Roll', 'dog', '2015-09-08 06:11:00');
INSERT INTO `animal_A` (`id`, `name`, `type`, `reg_date`) VALUES ('A352716', 'Back', 'cat', '2013-01-28 10:31:00');
-- animal_B
INSERT INTO `animal_B` (`id`, `name`, `type`, `reg_date`) VALUES ('A352413', 'Kevin', 'dog', '2019-01-13 17:50:00');
INSERT INTO `animal_B` (`id`, `name`, `type`, `reg_date`) VALUES ('A352714', 'Cherokee', 'dog', '2017-12-08 10:41:00');
INSERT INTO `animal_B` (`id`, `name`, `type`, `reg_date`) VALUES ('A352715', 'Roll', 'dog', '2015-10-08 08:18:00');
INSERT INTO `animal_B` (`id`, `name`, `type`, `reg_date`) VALUES ('A358711', 'Fall', 'cat', '2010-01-28 12:31:00');
```

![db_sql_join_2](/images/ComputerScience/Database/Sql/db_sql_join_2.JPG)


## INNER 조인

```
--Syntax
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
```

```
-- Inner Join
select 
  a.id, 
  a.name, 
  a.reg_date as a_date, 
  b.reg_date as b_date
from 
  animal_A as a join animal_B as b
on a.id = b.id;
```

![db_sql_join_innerjoin](/images/ComputerScience/Database/Sql/db_sql_join_innerjoin.JPG)

## LEFT OUTER 조인

```
--Syntax
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```

```
-- Left Join
select 
  a.id, 
  a.name, 
  a.reg_date as a_date, 
  b.reg_date as b_date
from 
  animal_A as a left join animal_B as b
on a.id = b.id;
```

![db_sql_join_leftjoin](/images/ComputerScience/Database/Sql/db_sql_join_leftjoin.JPG)

## RIGHT OUTER 조인

```
--Syntax
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
```

```
-- Right Join
select 
  a.id, 
  a.name, 
  a.reg_date as a_date, 
  b.reg_date as b_date
from 
  animal_A as a right join animal_B as b
on a.id = b.id;
```

![db_sql_join_rightjoin](/images/ComputerScience/Database/Sql/db_sql_join_rightjoin.JPG)


## FULL OUTER JOIN
* FULL OUTER JOIN은 왼쪽 (table1) 또는 오른쪽 (table2) 테이블 레코드와 일치하는 경우 모든 레코드를 리턴합니다.
* FULL OUTER JOIN과 FULL JOIN은 동일합니다.

```
--Syntax
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
```

## 카티전 조인 (cross 조인)
* 집합에서 __집합 곱의 개념__ 이다.
* A= {a, b, c, d} , B = {1, 2, 3} 일 때
* A CROSS JOIN B 는
* (a,1), (a, 2), (a,3), (b,1), (b,2), (b,3), (c, 1), (c,2), (c,3), (d, 1), (d, 2), (d,3)
* 와 같이 결과가 나타난다. 결과의 계수는 n(A) * n(B)  = 4 * 3 = 12 이다.

```
--Syntax
SELECT column_name(s)
FROM table1
CROSS JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
```

## 셀프 조인

```
--Syntax
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

[참고](https://futurists.tistory.com/17)


## JOIN ON과 WHERE의 차이점
아래와 같은 테이블이 있다고 하자.

```
CREATE TABLE test1 ( 
id VARCHAR(30) NOT NULL PRIMARY KEY,
name VARCHAR(30) NOT NULL
); 

CREATE TABLE test2 ( 
id VARCHAR(30) NOT NULL PRIMARY KEY,
name VARCHAR(30) NOT NULL
); 

INSERT INTO `test1` (`id`, `name`) VALUES ('1', 'a');
INSERT INTO `test1` (`id`, `name`) VALUES ('2', 'b');
INSERT INTO `test1` (`id`, `name`) VALUES ('3', 'c');

INSERT INTO `test2` (`id`, `name`) VALUES ('1', 'd');
INSERT INTO `test2` (`id`, `name`) VALUES ('2', 'e');
```

![db_sql_join_on_where_1](/images/ComputerScience/Database/Sql/db_sql_join_on_where_1.JPG)


```
-- TEST A
SELECT * 
FROM test1 as a left join test2 b
on a.id = b.id and b.name = 'd';
```

![db_sql_join_on_where_test_a](/images/ComputerScience/Database/Sql/db_sql_join_on_where_test_a.JPG)

```
-- TEST B
SELECT * 
FROM test1 as a left join test2 b
on a.id = b.id
where b.name = 'd';
```

![db_sql_join_on_where_test_b](/images/ComputerScience/Database/Sql/db_sql_join_on_where_test_b.JPG)

* __TEST A__ 경우는 __a 테이블과 b 테이블 중 b.name = 'd'인 경우__ 를 OUTER JOIN 한 결과가 나온다.
* __TEST B__ 경우는 __a와 b 테이블의 OUTER JOIN을 수행한 후__ 에 b.name = 'd'인 데이터들을 추출한다.
* 따라서, TEST A의 결과는 b.name = 'd'이 아닌 데이터도 존재, TEST B의 결과는 b.name = 'd'인 데이터만 존재한다.
* 이 점을 이용해서 __LEFT OUTER JOIN으로 차집합을 구현할 수 있다.__ 
* 오라클이나 MSSQL과 같은 경우는 EXCEPT 혹은 MINUS 등을 사용하면 되겠지만, MySQL은 버전에 따라 지원하는 경우도 있고 아닌 경우도 있다.

<br/>

## LEFT JOIN으로 차집합을 구현하기
* test1 테이블의 데이터 중 test2 테이블에 있는 데이터를 제외하고 가져오고 싶다. 
* 위의 테이블에서 JOIN하는 column을 기준으로 id=1, 2는 test2 테이블에도 있으니 제외하고, __id=3 | name=c 만을 가져오고 싶은 경우__ 이다.

```
select *
from test1 a left join test2 b
on a.id = b.id
where b.id is null;
```

![db_sql_join_on_where_test_c](/images/ComputerScience/Database/Sql/db_sql_join_on_where_test_c.JPG)

## 조인 활용 예시
* [JOIN 문제 풀어보기](https://programmers.co.kr/learn/courses/30/parts/17046)

## 참고
* [[W3S] SQL Join 참고 및 테스트](https://www.w3schools.com/sql/sql_join.asp)
* [참고 - JOIN 테스트](https://sqltest.net/#989125)