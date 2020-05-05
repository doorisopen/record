## MyBatis 란
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

## 주요 컴포넌트
* __spring-jdbc :__ 스프링이 제공하는 JDBC 래핑 모듈
* __MyBatis-Spring :__ 마이바티스가 제공하는 프레임워크 간의 연동 라이브러리
* __MyBatis :__ 마이바티스 프레임워크 모듈
* __mysql-connector-java :__ mysql 데이터베이스 JDBC 라이브러리
* __commons-dbcp :__ 커넥션 풀 지원 라이브러리 

### MyBatis 라이브러리
* __SqlSessionFactoryBuilder :__ MyBatis 설정 파일을 바탕으로 SqlSessionFactory를 생성
* __SqlSessionFactory :__ sqlSession 생성을 위한 컴포넌트
* __SqlSession :__ SQL 발행과 트랜잭션 관리
  + SqlSession 인터페이스는 MyBatis의 핵심 API 이다.

### myBatis-Spring 라이브러리
* __org.mybatis.spring.SqlSessionTemplate :__ sqlSession 인터페이스를 구현

### sqlSession을 사용하는 방법
* __sqlSession 객체를 DAO 객체에 의존관계 주입으로 사용__
* DAO 역할을 Mapper 객체를 통해 기능 제공
  + myBatis는 DAO의 구현을 자동으로 생성해주는 Mapper라는 기능을 제공
  + 개발자가 Mapper 인터페이스를 준비해서 Mapper 객체를 생성하도록 빈 정의

```
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

### Spring과 MyBatis의 의존 관계 흐름
![web_spring_mybatis_flow](/images/Web/Spring/web_spring_mybatis_flow.JPG)

### 응용 프로그램 시작시 수행 흐름
![web_spring_mybatis_flow1](/images/Web/Spring/web_spring_mybatis_flow1.JPG)

1. SqlSessionFactoryBean은 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청합니다.
2. 응용 프로그램은 SqlSessionFactoryBuilder를 사용하여 빌드된 SqlSessionFactory에서 SqlSession을 가져옵니다.
3. SqlSessionFactoryBuilder는 MyBatis 구성 파일의 정의에 따라 SqlSessionFactory를 생성합니다. 따라서 생성된 SqlSessionFactory는 Spring DI 컨테이너에 의해 저장됩니다.
4. MapperFactoryBean은 안전한 SqlSession(SqlSessionTemplate) 및 스레드 안전 매퍼 개체(Mapper 인터페이스의 프록시 객체)를 생성합니다.
따라서 생성되는 매퍼 객체는 스프링 DI 컨테이너에 의해 저장되며 서비스 클래스 등에 DI가 적용됩니다. 매퍼 개체는 안전한 SqlSession
(SqlSessionTemplate)을 사용하여 스레드 안전 구현을 제공합니다.

### 클라이언트 요청시 수행 흐름
![web_spring_mybatis_flow2](/images/Web/Spring/web_spring_mybatis_flow2.JPG)

1. 클라이언트가 응용 프로그램에 대한 프로세스를 요청합니다.
2. 애플리케이션(서비스)은 DI 컨테이너에서 주입한 매퍼 개체(매퍼 인터페이스를 구현하는 프록시 개체)의 방법을 호출합니다.
3. 매퍼 객체는 호출된 메소드에 해당하는 SqlSession (SqlSessionTemplate ) 메서드를 호출합니다.
4. SqlSession (SqlSessionTemplate )은 프록시 사용 및 안전한 SqlSession 메서드를 호출합니다.
5. 프록시 사용 및 스레드 안전 SqlSession은 트랜잭션에 할당된 MyBatis 표준 SqlSession을 사용합니다.
트랜잭션에 할당된 SqlSession이 존재하지 않는 경우 SqlSessionFactory 메서드를 호출하여 표준 MyBatis의 SqlSession을 가져옵니다.
6. SqlSessionFactory는 MyBatis 표준 SqlSession을 반환합니다. 반환된 MyBatis 표준 SqlSession이 트랜잭션에 할당되기 때문에 동일한 트랜잭션 내에 있는 경우 새 SqlSession을 생성하지 않고 동일한 SqlSession을 사용합니다. on 메서드를 호출하고 SQL 실행을 요청합니다.
7. MyBatis 표준 SqlSession은 매핑 파일에서 실행할 SQL을 가져와 실행합니다.