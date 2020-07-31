---
title: Spring과 MyBatis
category: Spring
date:   2020-06-04 00:30:59
comments: true
order: 2
---

## 목차
* MyBatis 란?
* MyBatis 장점
* MyBatis 주요 컴포넌트
* Spring에 MyBatis 설정하기
  + DataSource(JDBC, DBCP)
  + SqlSessionFactory와 SqlSession(MyBatis)
  + SqlSessionTemplate(MyBatis-Spring)
* sqlSession을 사용하는 방법
* Spring과 MyBatis의 의존 관계 흐름과 내부 동작 원리
  + Spring과 MyBatis의 의존 관계 흐름
  + 웹 애플리케이션 실행시 수행 흐름
  + 클라이언트 요청시 수행 흐름
* Question


## MyBatis 란?
* __SQL과 자바 객체를 매핑하는 사상에서 개발된 데이터베이스 접근용 프레임워크__
* SQL 기반으로 데이터베이스 접근을 수행하는 기존 방식과 큰 규모의 애플리케이션 개발에서 발생하는 과제를 해결하는 구조
* Mybatis는 모든 쿼리를 prepared statement로 실행한다.
  + prepared statement의 장점은 반복 실행 시 준비과정 없이 바로 실행해 좀 더 빠른 응답을 받을 수 있고 DBMS 입장에서는 CPU 사용률을 낮춰준다.


## MyBatis 장점
* __SQL의 체계적인 관리__ (설정 파일, 애노테이션)
  + 자바 Mapper 인터페이스를 통해 SQL 설정 파일과 연동
  + 비즈니스 로직에서 Mapper 인터페이스를 통해 SQL문 실행
* __자바 객체와 SQL 입출력 값의 바인딩__
* 동적 SQL 조합

## MyBatis 주요 컴포넌트
* __spring-jdbc :__ 스프링이 제공하는 JDBC 래핑 모듈
* __commons-dbcp :__ 커넥션 풀 지원 라이브러리
* __mysql-connector-java :__ mysql 데이터베이스 JDBC 라이브러리
* __MyBatis :__ 마이바티스 프레임워크 모듈
  + __SqlSessionFactoryBuilder :__ MyBatis 설정 파일을 바탕으로 SqlSessionFactory를 생성
  + __SqlSessionFactory :__ sqlSession 생성을 위한 컴포넌트
  + __SqlSession :__ SQL 발행과 트랜잭션 관리
    - SqlSession 인터페이스는 MyBatis의 핵심 API 이다.
* __MyBatis-Spring :__ 마이바티스가 제공하는 프레임워크 간의 연동 라이브러리
  + __org.mybatis.spring.SqlSessionTemplate :__ sqlSession 인터페이스를 구현



## Spring에 MyBatis 설정하기
SpringMVC에 MyBatis를 하나하나 설정해보면서 __각각 라이브러리가 어떠한 역할을하는지 분석__ 해보겠습니다. 

> 참고로 프로젝트 환경은 STS4, SpringMVC, Maven, junit(4.12) MySQL8(설치 필요) 다음과 같이 진행하였습니다.<br>
> 해당 소스코드가 궁금하시다면 [이곳 spring-mybatis](https://github.com/doorisopen/blog-source-code/tree/master/spring-mybatis)을 참고해주세요.

* __pom.xml에 추가한 dependency__
  + JDBC(spring-jdbc)
  + DBCP(commons-dbcp)
  + DB-Connector(mysql-connector-java)
  + MyBatis(mybatis)
  + MyBatis-Spring(mybatis-spring)
* __root-context.xml 설정한 내용__
  + DataSource
  + SqlSessionFactory
    - mybatis-config 설정
    - Mapper 설정
  + SqlSession
    - SqlSessionTemplate


#### DataSource 설정하기
__DataSource__ 는 데이터 액세스 기술 종류와 상관없이 __데이터베이스 접속을 관리해주는 인터페이스__ 입니다.

DataSource를 구현하기 위해서 서드 파티(ex, Apache Commons DBCP,Tomcat JDBC, BoneCP, HikariCP etc)가 제공하는 DataSource로 구현할 수 있습니다. 해당 프로젝트에서는 __Apache Commons DBCP(DataBase Connection Pool)__ 를 사용하였습니다. 또한, DataSource를 구현하기 위해서 위에서 pom.xml에 다음과 같은 dependency를 추가해주어야합니다.

```xml
<!-- JDBC -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
<!-- DBCP -->
<dependency>
  <groupId>commons-dbcp</groupId>
  <artifactId>commons-dbcp</artifactId>
  <version>1.4</version>
</dependency>
<!-- MySQL Connector -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.12</version>
</dependency>
```

* __JDBC(Java DataBase Connectivity)__ 는 DataBase와 연결하기 위한 Java 인터페이스 입니다.
  + spring-jdbc는 스프링이 제공하는 JDBC 래핑 모듈입니다.
* __DBCP(DataBase Connection Pool)__ 는 DataBase와 연결하고 있는 객체를 관리하기 위한 Connection Pool 입니다.
  + DBCP는 Connection Pool에서 오픈된 Connection을 가지고 있다가 필요한 곳에 Connection을 할당 및 관리하는 역할을 가졌습니다.
* __MySQL Connector__ 는 MySQL과 연결하기 위한 데이터베이스 JDBC 드라이버 입니다.

> 즉, __JDBC__ 는 DB와 연결하기 위해서 필요하고, __DBCP__ 는 Connection Pool을 이용해 효율성을 향상시키기 위해 필요합니다. 

위와 같이 컴포넌트를 추가했으면 root-context.xml에 dataSource 설정이 필요합니다.

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
  <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
  <property name="url" value="jdbc:mysql://127.0.0.1:3306/springdb?useSSL=false&amp;serverTimezone=UTC" />
  <property name="username" value="spring" />
  <property name="password" value="passwd" />
  <property name="maxActive" value="5" />
</bean>
```

dataSource 설정을 첫 줄부터 하나하나 집어보면

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
```
* `class="org.apache.commons.dbcp.BasicDataSource"`
  + 패키지 이름을 뜻합니다.
  + 아래 참고 확인.
* `destroy-method="close"`는 bean 객체의 스코프가 끝날을 경우(스프링에서는 어플리케이션 컨텍스트가 종료되었을 경우) class 속성에 선언한 클래스의 __close 메서드__ 를 호출하는 의미입니다.
  + close 메서드는 exception 처리와 connection close 처리 기능을 가지고 있습니다.

```xml
<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
<property name="url" value="jdbc:mysql://127.0.0.1:3306/springdb?useSSL=false&amp;serverTimezone=UTC" />
<property name="username" value="spring" />
<property name="password" value="passwd" />
<property name="maxActive" value="5" />
``` 

* 다음과 같은 설정을 __JNDI(Java Naming & Directory Interface)__ 이라고 합니다.
  + __JNDI__ 란 분산된 환경에서 특정한 이름으로 객체를 등록하고 관리할 수 있는 방법을 의미합니다.
  + __JNDI__ 는 객체를 네이밍 서비스에 등록, 삭제, 검색할 수 있는 방법을 제공합니다.
* `driverClassName` 는 __MySQL JDBC 드라이버__ 를 설정하는 속성입니다.
  + MySQL JDBC Driver 6 이상에서 com.mysql.cj 와 같이 cj가 포함합니다.
  + MySQL JDBC Driver 5 이하는 com.mysql.jdbc.Driver 다음과 같이 설정합니다.
* `url`, `username` `password` 는 __데이터베이스 연결 정보__ 를 설정하는 속성입니다.
* `maxActive`는 __dataSource 커넥션__ 관련 속성들 입니다.

| <center>속성 이름</center> | <center>설명</center> |
|:--------:|:--------:|
| initialSize | BasicDataSource 클래스 생성 후 최초로 getConnection() 메서드를 호출할 때 커넥션 풀에 채워 넣을 커넥션 개수 |
| maxActive | 동시에 사용할 수 있는 최대 커넥션 개수(기본값: 8) |
| maxIdle | 커넥션 풀에 반납할 때 최대로 유지될 수 있는 커넥션 개수(기본값: 8) |
| minIdle | 최소한으로 유지할 커넥션 개수(기본값: 0) |

> ※참고※ - JNDI<br>
> DB의 정보는 Java code에 직접 하드코딩하면 불편한 점이 많습니다.(유지보수 어려움) 계정정보와 같은 DB 정보가 바뀐다면 수정하고 재 컴파일하여 서버에 업로드해야 하기 떄문입니다. DB 정보를 외부(was)에서 관리하고 Source에서는 외부정보를 가져다쓸 수 있는 이름 값을 사용할 수 있게하는 방법이 JNDI입니다.


> ※참고※ - Commons DBCP<br>
> Commons DBCP 2에서는 패키지 이름이 org.apache.commons.dbcp에서 org.apache.commons.dbcp2로 변경되고 maxActive 속성의 이름이 maxTotal로 바뀌었다. Commons DBCP 2가 Commons DBCP 1.x와 하위 호환성을 보장하지 않아 Commons DBCP 1.x에서 Commons DBCP 2로 버전을 올릴 때 BasicDataSource 설정 코드를 그대로 사용할 수 없다.<br>
> 자세한 내용은 [d2.naver](https://d2.naver.com/helloworld/5102792) 에서 확인해주세요

##### DataSource 테스트 코드 작성하기

```java
package com.doop.mybatisproject;

import java.sql.Connection;
import javax.inject.Inject;
import javax.sql.DataSource;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "file:src/main/webapp/WEB-INF/spring/*.xml" })
public class DataSourceTest {

	@Inject
  private DataSource ds;
 
  @Test
  public void connection() throws Exception {
    try (Connection con = ds.getConnection()) {
      System.out.println("Connection Success: " + con + "\n");
    } catch (Exception e) {
      e.printStackTrace();
      System.out.println("Connection Fail\n");
    }
  }
}
```

위와 같이 테스트 코드를 작성하고 실행하면 정상적으로 DB에 연결되고 아래와 같이 연결정보가 출력되는것을 확인할 수 있습니다.

```
Connection Success: jdbc:mysql://127.0.0.1:3306/springdb?useSSL=false&serverTimezone=UTC, UserName=spring@localhost, MySQL Connector/J
```

#### MyBatis: SqlSessionFactory와 SqlSession
MyBatis를 구현하기 위해서 mybatis 컴포넌트를 다음과 같이 pom.xml에 추가해주어야 합니다.

```xml
<!-- MyBatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
```

MyBatis 구현에 필수적인 요소는 __SqlSessionFactory__ 와 __SqlSession__ 가 있습니다. 이들은 MyBatis 컴포넌트안에 있는 인터페이스입니다.(기능은 SqlSessionManager이 상속받아 구현)

* org.apache.ibatis.session
* __SqlSessionFactory :__ sqlSession 생성을 위한 컴포넌트입니다.
* __SqlSession :__ SQL 발행과 트랜잭션 관리를 담당합니다.
  + MyBatis의 SqlSession은 Thread Safe 하지 않습니다.(이를 위한 해결책은 SqlSessionTemplate)

```xml
<!-- (2) -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"></property>
  <property name="configLocation" value="classpath:/mybatis-config.xml"></property>
  <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"></property>  
</bean>
```

위는 root-context.xml의 sqlSessionFactory에 대한 설정내용입니다. sqlSessionFactory는 __sqlSession을 생성하기 위한 정보__ 들을 속성으로 가지고 있습니다.

* `dataSource` DataBase 연결 정보
* `configLocation` MyBatis 설정 파일 위치
* `mapperLocations` SQL이 기술된 매퍼파일의 위치

##### SqlSessionFactory와 SqlSession 테스트 코드 작성하기

```java
package com.doop.mybatisproject;

import javax.inject.Inject;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "file:src/main/webapp/WEB-INF/spring/*.xml" })
public class MyBatisTest {
	
	@Inject
	private SqlSessionFactory sqlsessionfactory;
	
	@Test
	public void SqlSessionFactoryTest() throws Exception {
		System.out.println("SqlSessionFactory: "+sqlsessionfactory);
	}
	
	
	@Test
	public void SqlSessionTest() throws Exception {
		try (SqlSession session = sqlsessionfactory.openSession()){
			System.out.println("session: "+session);
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}
}
```

위와 같이 테스트 코드를 작성하고 실행하면 아래와 같이 세션정보가 출력되는것을 확인할 수 있습니다.

```
SqlSessionFactory: org.apache.ibatis.session.defaults.DefaultSqlSessionFactory@2b30a42c
session: org.apache.ibatis.session.defaults.DefaultSqlSession@723ca036
```

#### MyBatis-Spring: SqlSessionTemplate
SqlSessionTemplate는 MyBatis-Spring 컴포넌트 안에 있는 클래스입니다. 

```xml
<!-- MyBatis-Spring -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.2</version>
</dependency>
```

##### MyBatis-Spring는 무엇인가?
__MyBatis-Spring__ 은 MyBatis를 기반으로 기능을 확장한 모듈로, MyBatis와 Spring을 연동해주는 모듈입니다.

##### MyBatis-Spring와 MyBatis 컴포넌트의 차이?
MyBatis-Spring와 MyBatis의 차이는 DB 작업에 필요한 SqlSession에 있습니다. MyBatis에 SqlSession은 기본적으로 Thread-Safe 하지 않습니다. 따라서 요청이 들어오면 각 요청마다 Thread를 생성해 주어야 합니다.

하지만 MyBatis-Spring은 MyBatis를 기반으로 기능을 확장한 모듈로 __SqlSessionTemplate(SqlSession 인터페이스를 확장) 클래스를 제공하는데 이때 는 Thread-Safe 합니다.__

```xml
<!-- (3) -->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
  <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
</bean>
```

다음과 같이 SqlSessionTemplate에 SqlSessionFactory를 주입해 줍니다. 그 이유는 __SqlSessionFactory는 sqlSession 생성을 위한 정보를 가지고 있기 때문입니다.__

## sqlSession을 사용하는 방법
* __sqlSession 객체를 DAO 객체에 의존관계 주입으로 사용__
* DAO 역할을 Mapper 객체를 통해 기능 제공
  + myBatis는 DAO의 구현을 자동으로 생성해주는 Mapper라는 기능을 제공
  + 개발자가 Mapper 인터페이스를 준비해서 Mapper 객체를 생성하도록 빈 정의

```java
@Repository
public class MemberDAOImpl implements MemberDAO {
    @Autowired
    private SqlSession sqlSession;

    private static final String namespace = "org.doop.myweb.mapper.StudentMapper";

    public void add(StudentVO vo) throws Exception {
        sqlSession.insert(namespace + ".insert", vo);
    }
    ...(생략)
```

## Spring과 MyBatis의 의존 관계 흐름과 내부 동작 원리

#### Spring과 MyBatis의 의존 관계 흐름
![web-spring-mybatis-flow]({{ site.baseurl }}{{ site.spring_img }}/web-spring-mybatis-flow.JPG)

#### 응용 프로그램 시작시 수행 흐름
![web-spring-mybatis-flow1]({{ site.baseurl }}{{ site.spring_img }}/web-spring-mybatis-flow1.JPG)

1. SqlSessionFactoryBean은 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청합니다.
2. 응용 프로그램은 SqlSessionFactoryBuilder를 사용하여 빌드된 SqlSessionFactory에서 SqlSession을 가져옵니다.
3. SqlSessionFactoryBuilder는 MyBatis 구성 파일의 정의에 따라 SqlSessionFactory를 생성합니다. 따라서 생성된 SqlSessionFactory는 Spring DI 컨테이너에 의해 저장됩니다.
4. MapperFactoryBean은 안전한 SqlSession(SqlSessionTemplate) 및 스레드 안전 매퍼 개체(Mapper 인터페이스의 프록시 객체)를 생성합니다.
따라서 생성되는 매퍼 객체는 스프링 DI 컨테이너에 의해 저장되며 서비스 클래스 등에 DI가 적용됩니다. 매퍼 개체는 안전한 SqlSession
(SqlSessionTemplate)을 사용하여 스레드 안전 구현을 제공합니다.

#### 클라이언트 요청시 수행 흐름
![web-spring-mybatis-flow2]({{ site.baseurl }}{{ site.spring_img }}/web-spring-mybatis-flow2.JPG)

1. 클라이언트가 응용 프로그램에 대한 프로세스를 요청합니다.
2. 애플리케이션(서비스)은 DI 컨테이너에서 주입한 매퍼 개체(매퍼 인터페이스를 구현하는 프록시 개체)의 방법을 호출합니다.
3. 매퍼 객체는 호출된 메소드에 해당하는 SqlSession (SqlSessionTemplate ) 메서드를 호출합니다.
4. SqlSession (SqlSessionTemplate )은 프록시 사용 및 안전한 SqlSession 메서드를 호출합니다.
5. 프록시 사용 및 스레드 안전 SqlSession은 트랜잭션에 할당된 MyBatis 표준 SqlSession을 사용합니다.
트랜잭션에 할당된 SqlSession이 존재하지 않는 경우 SqlSessionFactory 메서드를 호출하여 표준 MyBatis의 SqlSession을 가져옵니다.
6. SqlSessionFactory는 MyBatis 표준 SqlSession을 반환합니다. 반환된 MyBatis 표준 SqlSession이 트랜잭션에 할당되기 때문에 동일한 트랜잭션 내에 있는 경우 새 SqlSession을 생성하지 않고 동일한 SqlSession을 사용합니다. on 메서드를 호출하고 SQL 실행을 요청합니다.
7. MyBatis 표준 SqlSession은 매핑 파일에서 실행할 SQL을 가져와 실행합니다.

## Question
* MyBatis란 무엇인가요?
* MyBatis의 동작 흐름을 설명해 주세요


## Reference
* 학부 웹 프레임워크 강의자료
* [jwkim96.tistory.com/62](https://jwkim96.tistory.com/62)
* [velog.io/@wimes/2](https://velog.io/@wimes/2.-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-Spring-MyBatis-MySQL%EC%9D%98-%EC%84%A4%EC%A0%95-2zk4cf5gof#mybatis%EC%99%80%EC%9D%98-%EA%B5%AC%EC%84%B1)
* [오 좋은 정보인걸](https://nastyle.tistory.com/15)
* [ONE.0의 공부노트](https://one0.tistory.com/15)
* [gmlwjd9405.github.io/](https://gmlwjd9405.github.io/2018/05/15/setting-for-db-programming.html)