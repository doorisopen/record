---
title: GROUP BY와 HAVING
category: SQL
comments: true
order: 1
---

# GROUP BY와 HAVING
HAVING과 GROUP BY은 다음과 같은 syntax를 따릅니다.

```
SELECT column-names
  FROM table-name
 WHERE condition
 GROUP BY column-names
HAVING condition
```


예를 들어 다음과 같은 테이블이 있다고 하자.

```
SELECT *
FROM member;
```

![db-sql-groupby_having_1]({{ site.baseurl }}/images/ComputerScience/Database/Sql/db-sql-groupby_having_1.JPG)

## GROUP BY

* MySQL에서의 GROUP BY는 특정 컬럼 이름을 지정(column-names)해주면 그 컬럼의 UNIQUE한 값에 따라서 데이터를 그룹 짓고, 중복된 열은 제거됩니다.
* GROUP BY는 보통 집합 함수(aggregate function, [AVG, SUM, COUNT 등을 말합니다])와 같이 쓰이며, 다음과 같은 형태를 지닙니다.

```
SELECT
    c1, c2,..., cn, aggregate_function(ci)
FROM
    table
WHERE
    where_conditions
GROUP BY c1 , c2,...,cn;
```

### GROUP BY 예제
테이블에서 team 별로 그룹짓고 각 team(1,2,3,5)에 몇명이 있는지 count 하는 예제입니다. 

```
SELECT team, count(*)
FROM member
GROUP BY team;
```

결과는 다음과 같습니다.

![db-sql-groupby_having_2]({{ site.baseurl }}/images/ComputerScience/Database/Sql/db-sql-groupby_having_2.JPG)

## HAVING
* HAVING은 간단하게 생각해서 GROUP BY한 결과에 조건을 붙이고 싶을때, 즉 GROUP BY의 WHERE 절과도 같다고 볼 수 있습니다.

### HAVING 예제
위의 예제에서, 각각 team 안에 2명 이상인 경우를 예제로 보여드리겠습니다.

``` 
SELECT team, count(*)
FROM member
GROUP BY team
HAVING count(*) > 2;
```

결과는 다음과 같습니다.

![db-sql-groupby_having_3]({{ site.baseurl }}/images/ComputerScience/Database/Sql/db-sql-groupby_having_3.JPG)

## 참고
[SQL 테스트 해보기](https://sqltest.net/#988473)