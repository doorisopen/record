# story10. 로그는 반드시 필요한 내용만 찍자
생성일: 2022년 6월 22일 오후 11:06

---

# System.out.println()의 문제점

가장 편하고 확인하기 좋은 방법이지만 성능에 영향을 많이 주는 경우가 빈번히 발생하는 방법이다.

- 커널 CPU를 많이 사용한다
- 윈도우 화면에 출력할 때 커널 CPU를 많이 점유한다.

내용이 완전히 프린트되거나 저장될 때까지, 뒤에 프린트하려는 부분은 대기하게 된다. 그러면 애플리케이션에서는 대기 시간이 발생한다. 이 대기 시간은 시스템의 속도에 의존적이다.

# System.out.format() 메서드

JDK 5.0의 System 클래스에서 사용하는 out 객체 클래스인 PrintStream에 새로 추가된 메서드

- format(String format, Object …args): 지정된 포멧으로 프린트를 한다. 뒤에 있는 매개변수를 쉼표로 나열하도록 되어 있다.
- format(Locale l, String format, Object …args): 위의 메서드와 동일하되 가장 앞에 지역 정보를 포함한다. 지역에 따라 다른 형태의 데이터를 프린트할 수 있다.
- String append 보다 성능은 낮다.
- 개발 시점에 디버그용 로그로 사용하고 운영 시 제거하는 경우에 사용하는 것이 좋다.

# 로그를 더 간결하게 처리하는 방법

필드 변수로 로그 제어하는 방법

```java
public class ServiceSample {
		private final boolean printFlag = false;

		public void get() {
				if (printFlag) {
						// print()
				}
		}
}
```

로그 클래스 생성 후 static final 변수 활용

```java
public class LogSample {
		public static final boolean printFlag = false;
}

if (LogSample.printFlag) {
}
```

로그 공통 클래스 활용

```java
public class LogSample {
		private static final boolean printFlag = false;
		
		public static void log(String message) {
			if (printFlag) {
					// print(message)
			}
		}
}

```

# 로그 사용 시의 문제점

- 디버그용 로그 메시지는 간단한 문자든 쿼리든 상관 없이 하나 이상의 객체가 필요하다.
- 객체를 생성하는 데 메모리와 시간이 소요된다.
- 메모리에서 제거하기 위해서 GC를 수행해야하고 GC 수행 시간이 소요 된다.

```java
logger.info("hello" + message);
```

위와 같이 처리하는 경우 info() 안에 있는 값들을 문자열로 변환하는 작업이 반드시 수행된 다음, 메소드 호출이 진행된다.

> 디버그용 로그를 제거하는 것이 성능 개선에 가장 좋다.
> 

# Slf4j, Logback

간단히 로그를 처리해 주는 프레임워크

```java
log.info("hello {}", "world");
```

위는 slf4j 로그를 사용한 예시 이다.

코드를 살펴보면 문자열에 중괄호를 넣고 출력할 데이터를 콤마로 구분하여 순서대로 기입하여 전달한다.

- 로그를 출력하지 않을 경우 필요 없는 문자열 더하기 연산이 발생하지 않는다.

## Logback

- 예외의 스택 정보를 출력할 때 해당 클래스가 어떤 라이브러리를 참고하고 있는지 포함하여 제공해준다.

### Logback 탈취 이슈

- [https://blog.alyac.co.kr/4361](https://blog.alyac.co.kr/4361)
- [https://foxydog.tistory.com/89](https://foxydog.tistory.com/89)
- [https://cheezred.tistory.com/203](https://cheezred.tistory.com/203)

# 예외 처리는 이렇게

예외 처리할 때도 성능에 많은 영향을 줄 수 있다.

```java
try {
		//..
} catch (Exception e) {
		e.printStackTrace();
}
```

일반적으로 위와 같이 예외 처리를 하지만 e.printStackTrace() 에서 스택 정보를 확인하고, 확인된 정보를 콘솔에 프린트한다.

- 콘솔에 찍힌 이 로그를 알아보기가 힘들다.
    - 왜냐하면 여러 스레드에서 콘솔에 로그를 프린트하면 데이터가 섞이기 때문이다.
- 자바의 예외 스택 정보는 로그를 최대 100개까지 프린트하기 때문에 서버의 성능에도 많은 부하를 준다.
- 스택정보를 가져오는 부분은 거의 90% 이상이 CPU를 사용하는 시간이다.
    - 나머지 프린트하는 부분은 대기 시간이 소요된다.

스택 정보를 보고 싶은 경우 다음과 같이 처리하는 방법이 있다.

```java
try {
...
} catch (Exception e) {
		StackTraceElement[] ste = e.getStackTrace();
		String className = ste[0].getClassName();
		String methodName = ste[0].getMethodName();
		String lineNumber = ste[0].getLineNumber();
		String fileName = ste[0].getFileName();
		log.severe("Exception=" + e.getMessage());
		log.severe(className + "." + methodName + " " + fileName + " " + lineNumber + " line");
}
```