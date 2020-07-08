---
title: JDBC(Java DataBase Connectivity) 동작 순서
category: Web
date:   2020-06-04 00:30:59
comments: true
order: 3
---

## JDBC
![web-jdbc]({{ site.baseurl }}/images/Web/web-jdbc.JPG)

## 1단계 : JDBC 드라이버 로딩
* JDBC API 사용을 위해서는 먼저 JDBC 규격에 따른 실제 구현된 각 JDBC 드라이버 클래스를 로딩해야 한다.
* JDBC 드라이버 로딩은 크게 __두 가지__ 가 있다.
  + jdbc.drivers 환경변수 이용
  + Class.forName() 메서드 이용
    - __일반적으로는 이용하는 방법이다.__ 이 경우 원하는 JDBC 드라이버를 직접 프로그램 코드에서 로드할 수 있게 된다

```java
//==jdbc.drivers 환경변수 이용하기==//
System.setProperty("jdbc.drivers","com.mysql.jdbc.Driver");

//==Class.forName() 메서드 이용하기==//
Class.forName("com.mysql.jdbc.Driver"); // MySQL JDBC Driver 5 이하
Class.forName("com.mysql.cj.jdbc.Driver"); // MySQL JDBC Driver 6 이상
```

* __com.mysql.jdbc.Driver는 mySQL 의 JDBC 드라이버 클래스 이름이다.__
* 데이터베이스마다 클래스 이름이 다르므로 해당 DB의 이름을 정확하게 넣어줘야 한다.
* 오라클의 JDBC 드라이버 클래스 이름 - oracle.jdbc.driver.OracleDriver

## 2단계: 데이터베이스 연결
* 드라이버가 로드되면 JDBC API를 이용해 프로그램을 작성할 수 있음을 의미한다
* 실제 데이터베이스 프로그래밍을 위해서는 먼저 DB에 연결해야 하는데 DriverManager 클래스의 getConnection() 메서드를 사용한다.

```java
//==기본 형식==//
jdbc:mysql://IP주소/스키마:PORT(옵션)
```

* __IP 주소:__  MySQL 데이터베이스가 설치된 컴퓨터의 IP 주소, 또는 도메인 이름이다.
* __스키마:__ 데이터베이스에서 생성한 스키마(데이터베이스) 이름이다.
* __포트:__ 기본 설정 값인 3306 포트를 사용하는 경우에는 생략할 수 있다.

```java
// MySQL 5.6 이하
String jdbc_url = "jdbc:mysql://localhost/jspdb";
// MySQL 5.7 이상
String jdbc_url = "jdbc:mysql://localhost/jspdb?useSSL=false&serverTimezone=UTC";

Connection conn = DriverManager.getConnection(jdbc_url ,"jspbook","passwd");
```

## 3단계: Statement / PreparedStatement 생성
* __Statement__ 는 데이터베이스 연결로 부터 SQL 문을 수행할 수 있도록 해주는 클래스
  + executeQuery()
    - SELECT문을 수행할 때 사용한다. 반환 값은 ResultSet 클래스의 인스턴스로, 해당 SELECT문의 결과에 해당하는 데이터에 접근할 수 있는 방법을 제공한다.
  + executeUpdate()
    - UPDATE, INSERT, DELETE와 같은 문을 수행할 때 사용한다. 반환 값은 int 값으로, 처리된 데이터의 수를 반환한다. 
* Statement 객체는 쿼리를 문자열로 연결하므로 소스가 복잡하고 오류가 발생하기 쉽다

```java
Statement stmt = conn.createStatement();
stmt.executeUpdate("insert into test values
(' "+request.getParameter("username")+" ','"+request.getParameter("email")+" ')");
```

* __PreparedStatement 는__ SQL 에 필요한 변수 데이터를 "?"로 표시하고 메서드를 통해 설정하는 방식으로 Statement 보다 구조적이고 편리해 권장되는 방법이다.

```java
PreparedStatement pstmt = conn.prepareStatement("insert into test values(?,?)");
pstmt.setString(1,request.getParameter("username");
pstmt.setString(2,request.getParameter("email");
pstmt.executeUpdate();

//==리소스 명시적 반납==//
stmt.close();
pstmt.close();
```

## 4단계: SQL문 전송
* 데이터 조합과 함께 만들어진 SQL 문은 명시적인 처리 명령에 의해 DB로 전달되어 실행된다.
* __insert, delete, update__ 와 같이 데이터 변경이 있는 쿼리의 경우에는 다음과 같이 __executeUpdate()__ 문을 사용한다.
* __select__ 문의 경우 __executeQuery() 메서드__ 를 사용하고 조회 결과를 받기 위해
ResultSet 객체를 사용한다.

```java
pstmt.executeUpdate();
int count = pstmt.executeUpdate(); // 처리한 로우의 개수 반환
```

## 5단계: 결과 받기
* __데이터베이스에서 조회한 결과를 받기 위해서는 Statement 나 PreparedStatement의 executeQuery()를 사용한다.__
* 조회 결과는 ResultSet 객체로 리턴되며 ResultSet은 실제 데이터 셋이 아니라 데이터에 접근할 수 있는 포인터의 집합 개념으로 이해하면 된다.

<br />

* ResultSet 을 이용해 데이터를 처리하는 방법은 __rs.next()__ 메서드로 다음 데이터를 확인하고 데이터가 있을 경우 __rs.getXxx()__ 메서드를 이용해 특정 컬럼에 해당하는 데이터를 가지고와 사용하게 된다.
* getXxx() 메서드는 데이터 타입별로 존재하므로 컬럼 데이터 타입에 따라 적절히 사용한다.

```java
ResultSet rs = pstmt.executeQuery();
while(rs.next()) {
  name = rs.getString(1); // or rs.getString("name");
  age = rs.getInt(2); // or rs.getInt("email");
}
rs.close();
```

## 6단계: 연결 해제
* 사용이 끝난 데이터베이스 연결은 __conn.close()__ 메서드를 이용해 닫아 주도록 한다. DB 연결은 중요한 자원이고 제한적이므로 사용이 끝난 연결은 반드시 해제해 주어야 하므로 주의 하도록 한다. 

## Reference
* 모교 웹 프로그래밍 강의 요약