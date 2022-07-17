# story12. DB를 사용하면서 발생 가능한 문제점들
생성일: 2022년 6월 26일 오후 6:36

# DB Connection, Connection Pool, DataSource

## DB Connection Pool

Connection 객체를 생성하는 부분에서 발생하는 **대기 시간**을 줄이고, 네트워크의 부담을 줄이기 위해서 사용하는 것이 **DB Connection Pool**이다.

지금은 모든 WAS에서 Connection Pool을 제공하고, DataSource를 사용하여 JNDI로 호출해 사용할 수 있다.

> **DataSource와 DB Connection Pool의 차이**
DataSource는 JDK 1.4 버전부터 생긴 표준이다.
Connection Pool로 연결을 관리해야하고, 트랜잭션 관리도 가능하도록 만들어야한다. 그러므로 **DataSource가 DB Connection Pool을 포함**한다고 생각하면 된다.
다만, **DB Connection Pool은 자바 표준으로 지정되어 있는 것이 없다.** 따라서, WAS 벤더에 따라서 사용법이 많이 상이할 수 있다. 그러나 **DataSource는 자바 표준**이므로 WAS에 상관없이 사용법은 동일하다.
> 

# Statement

**Statement**와 거의 동일하게 사용할 수 있는 Statement 인터페이스의 자식 클래스로 **PreparedStatement**가 있다. 그리고 PL/SQL을 처리하기 위해서 사용하는 PreparedStatement의 자식 클래스로 **CallableStatement**가 있다.

### **Statement와 PreparedStatement의 차이점**

- 캐시(Cache) 사용 여부

**수행 프로세스**

1. 쿼리 문자 분석
2. 컴파일
3. 실행

**Statement**를 사용하면 매번 쿼리를 수행할 때마다 1~3번의 단계를 수행한다.

**PreparedStatement**는 처음 한 번만 1~3번 단계를 수행하고 캐시에 담아서 재사용한다.

따라서, PreparedStatement가 DB에 훨씬 적은 부하를 주며, 성능도 좋다. 또한, 쿼리에서의 변수를 ‘’로 묶어서 처리하지 않고 ?로 매핑하여 처리하기 때문에 가독성뿐 아니라 보안상 좋다.

# DB 사용 시 주의점

DB 사용 시, 사용한 객체의 자원을 해제해줘야 한다.

보통 DB 관련 객체를 얻는 순서는 다음과 같다.

1. Connection
2. Statement, PreparedStatement
3. ResultSet

객체를 닫는 순서는 위의 역순이다.

ResultSet > Statement, PreparedStatement > Connection

## 객체(ResultSet, Statement, Connection)가 닫히는 경우

- close() 메서드를 호출한 경우
- GC 대상이 되어 GC된 경우

객체 자원을 해제 하지 않고 GC가 될 때 까지 기다리지 않고 빠르게 해제하는 것이 DB 서버의 부담이 적어지게 된다.

### Connection

- 치명적인 에러가 발생하는 경우

Connection 객체의 경우 **Connection Pool**을 사용하여 관리된다.

시스템이 기동되면 지정된 개수만큼 연결하고, 필요할 때 증가시키도록 되어있다. 증가되는 최대 값 또한 지정하도록 되어 있다. **사용자가 증가해 더 이상 사용할 수 있는 연결이 없으면, 여유가 생길 때까지 대기한다. 그러다가 어느 정도 시간이 지나면 오류가 발생한다.**

때문에, **GC가 될 때까지 기다리면 Connection Pool이 부족해지는 것은 시간 문제**이다. 그래서 쿼리 수행 후에 DB 관련 **객체의 자원을 close() 메소드를 호출하여 자원을 해제**해줘야 한다.

```java
Connection conn = null
PreparedStatement ps = null;
ResultSet rs = null;
try {
	conn = 
  ps = 
	rs =
} catch () {
} finally {
	try { rs.close(); } catch(Exception rse) {}
	try { ps.close(); } catch(Exception pse) {}
	try { conn.close(); } catch(Exception coe) {}
}
```

> **ResultSet API 인터페이스 내용 중…**
’ResultSet 객체는 현재 열의 데이터를 가리키는 커서를 관리한다.’ 즉, 데이터는 갖고 있지 않고, 커서만을 관리하는 객체다. 

: 해당 인터페이스를 구현한 JDBC를 제공하는 벤더에 따라 구현 내용이 달라질 수 있다는 의미
> 

# AutoClosable 인터페이스

JDK 7 부터 등장한 java.lang 패키지의 AutoClosable 인터페이스

해당 인터페이스에는 void close() 메서드 단 한 개만 선언되어있다.

**close() 설명**

- try-catch-resources 문장으로 관리되는 객체에 대해서 자동적으로 close() 처리를 한다.
- InterruptedException을 던지지 않도록 하는 것을 권장한다.
- 이 close() 메서드를 두 번 이상 호출할 경우 뭔가 눈에 보이는 부작용이 나타나도록 해야 한다.

예를 들어 파일을 읽는 작업을 수행하는 메소드를 다음과 같이 정의한다고 하면

```java
public String readFile(String fileName) throws IOException {
	FileReader reader = new FileReader(new File(fileName));
	BufferedReader br = new BufferedReader(reader);
	String data = null;
	try {
		data = br.readLine();
	} catch (Exception e) {
		...
	} finally {
		try { br.close(); } catch(Exception bre) {}
	}
	return data;
}
```

JDK 7부터는 아래와 같이 try 블록시 시작될 때 소괄호 안에 close() 메서드를 호출하는 객체를 생성해 주면 간단히 처리할 수 있다.

```java
public String readFile(String fileName) throws IOException {
	FileReader reader = new FileReader(new File(fileName));
	String data = null;
	try(BufferedReader br = new BufferedReader(reader)) {
		data = br.readLine();
	} catch (Exception e) {
		...
	}
	return data;
}
```

**close() 메서드 호출 대상이 여러 개라면 세미콜론으로 구분하여 작성**하면 된다.

# JDBC 사용시 팁

- setAutoCommit() 사용 자제
    - 여러 개의 쿼리를 동시에 작업할 때 성능에 영향을 줄 수 있다.
- 배치성 작업은 executeBatch() 메서드 사용
    - 배치성 작업은 Statement 인터페이스에 정의된 addBatch()를 사용하여 쿼리를 지정하고 executeBatch()를 사용하여 쿼리를 수행하면 여러 개의 쿼리를 한 번에 수행할 수 있어 JDBC 호출 횟수가 감소되어 성능이 좋아진다.
- setFetchSize() 메서드로 데이터를 더 빠르게 가져올 수 있다.
    - 한 번에 가져오는 열의 개수는 JDBC의 종류에 따라 다르지만 데이터 수가 정해져 있는 경우 Statement와 ResultSet 인터페이스에 있는 setFetchSize() 메서드를 사용하여 원하는 개수를 정의하자
    - 다만 너무 많은 건수를 지정하면 서버에 많은 부하가 올 수 있다.