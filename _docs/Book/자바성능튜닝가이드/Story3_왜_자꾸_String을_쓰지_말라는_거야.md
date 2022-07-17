# Story3. 왜 자꾸 String을 쓰지 말라는 거야
생성일: 2022년 6월 11일 오후 5:35

---

# String 클래스를 잘못 사용한 사례

```java
String sql = "";
sql += "select * ";
sql += "from ( ";
sql += "select a.col, ";
...
```

요즘에는 Mybatis나 Hibernate와 같은 라이브러리를 사용하지만, 과거에는 쿼리문을 위와 같은 방식으로 작성하였다. 그런데 이렇게 쿼리를 작성하면, 개발 시에는 편하지만 **메모리를 많이 사용하게 되는 문제점**이 있다.

그래서 문자열을 다룰 땐 String 클래스를 사용하기 보다 아래와 같이 StringBuilder, StringBuffer 클래스를 사용하는 것이 좋다.

```java
StringBuilder sql = new StringBuilder();
sql.append("select * ");
sql.append("from ( ");
sql.append("select a.col, ");
...
```

# StringBuffer, StringBuilder 클래스

JDK 5.0 기준으로 문자열을 만드는 클래스 `String`, `StringBuffer`, `StringBuilder`(JDK 5.0 추가)가 가장 많이 사용된다.

- `StringBuffer` 클래스
    - Thread Safe하게 설계되었다.
    - 여러 개의 Thread가 하나의 StringBuffer 객체를 처리해도 전혀 문제가 되지 않는다.
- `StringBuilder` 클래스
    - Thread Safe하지 않는다.
    - 단일 Thread에서의 안전성을 보장한다.

# String, StringBuffer, StringBuilder 클래스

```java
String input = "abcd";
String strTest1 = "";
for (int count = 0; count < 1000000; count++) {
		strTest1 += input;
}

StringBuffer strTest2 = new StringBuffer();
for (int count = 0; count < 1000000; count++) {
		strTest2.append(input);
}

StringBuilder strTest3 = new StringBuilder();
for (int count = 0; count < 1000000; count++) {
		strTest3.append(input);
}
```

String 클래스를 사용하여 문자열을 다룰 경우(약 100만번 수행 시), StringBuffer, StringBuilder에 비해 속도는 약 367배 느리고 있고 메모리는 약 3000배 정도 많이 사용한다.

이렇게 차이나는 이유가 무엇일까? 

그 이유는 `strTest1 += input` 연산을 수행하게 되면 **새로운 String 클래스의 객체가 생성되고 기존에 메모리에 올라간 strTest1 객체는 쓰레기 값이 되어 GC 대상이 되어버린다.**

GC를 많이 사용하게 되면 시스템의 CPU를 사용하게 되고 시간도 많이 소요된다. 그래서 개발 시 메모리 사용을 최소화하며 개발하는 것이 좋다.

# 결론

- String
    - 짧은 문자열을 더할 경우 사용
- StringBuffer
    - Thread Safe가 필요한 프로그램 혹은 잘 모를 경우 사용
    - 클래스에 static 으로 선언된 문자열을 변경, singleton으로 선언된 클래스에 선언된 문자열일 경우 사용
- StringBuilder
    - Thread Safe 가 필요하지 않는 경우 사용
    

<aside>
💡 JDK 5.0 이상을 사용함면 컴파일러가 자동으로 StringBuilder로 변환 시켜 준다.

</aside>