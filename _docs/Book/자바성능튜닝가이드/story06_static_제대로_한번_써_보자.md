# story6. static 제대로 한번 써 보자
생성일: 2022년 6월 15일 오후 10:18

---

변수에 `static` 키워드를 선언하면 **클래스 변수**라고 한다.

```java
public class Member {
	private static int bornDay = 10;
	private String name;
}
```

bornDay는 **‘객체의 변수’**가 되는 것이 아니라 **‘클래스의 변수’**가 된다. 

만약 Member 인스턴스를 100개를 만들어도 **bornDay 변수에 대해서는 동일한 주소 값을 참조**한다.

```java
public class Member {
	private String name;

	static {
		name = "hello";
		name = "bye";
	}
}
```

클래스에는 위와 같이 static 키워드로 **초기화 블록**을 정의할 수 있다.

**초기화 블록**은 순차적으로 읽혀진다.

## static 의 특징

- 다른 JVM에서 static을 선언해도 다른 주소나 다른 값을 참조한다.
    - 하나의 JVM이나 WAS 인스턴스에서는 같은 주소에 존재하는 값을 참조한다는 의미이다.
- GC의 대상이 되지 않는다.

# Static 활용하기

- 자주 사용하고 잘 변하지 않는 변수는 static final 으로 선언한다.
- 설정 파일 정보는 static으로 관리한다.
- 코드성 데이터는 DB에서 한 번만 읽는다.
    - 변경 가능성이 적은 데이터(회사 코드 등)은 DB에서 한번만 읽고 관리

> static 을 활용해서 변경 가능성이 적은 코드 데이터를 관리하다 보면, 간혹 수정되는 경우 **서로 다른 JVM 에서 코드 정보가 상이해 질 수 있다.** 이를 위한 대안으로 **mem-cached, EhCache 등의 캐시(Cache)를 많이 사용**한다.
> 

## static 사용 주의점

```java
public class LottoGame {
	private static boolean isWinner;

	public boolean checkWinning() {
		isWinner = ...;// 당첨 여부 확인 로직
		return isWinner();
	}

	public boolean isWinner() {
		return isWinner;
	}
}
```

당첨 여부 관리하는 변수를 static 으로 선언했다. 

그러나 위 코드는 **여러 스레드가 접근하면 심각한 문제(동시성 문제)가 발생**할 수 있다.

static 으로 선언하면 객체의 변수가 아닌 클래스 변수이기 때문에 여러 스레드가 접근하여도 동일한 참조 값을 가지게 된다. 때문에 특정 스레드가 당첨 확인(checkWinning())을 수행하는 동시에 당첨자(isWinning())를 확인한다면 전혀 다른 결과를 확인하게 된다.

## static과 메모리 릭

static 으로 선언된 부분은 **GC가 수행되지 않는다.**

만약 Collection을 static으로 선언하고 지속적으로 데이터가 쌓여도 GC가 수행되지 않기 때문에 결국 OutOfMemoryError가 발생하게 된다. 즉, 시스템을 재시작해야 하며 해당 애플리케이션 인스턴스는 서비스를 할 수 없다.

**더 이상 사용 가능한 메모리가 없어지는 현상**을 `**메모리 릭(Memory Leak)**`이라고 한다.

메모리 릭은 **static과 Collection 객체를 잘못 사용하면 발생**한다.

> 메모리 릭의 원인은 메모리의 현재 상태를(메모리의 단면을) 파일로 남기는 HeapDump라는 파일을 통해서 확인 가능하다. JDK/bin 디렉터리에 있는 jmap이라는 파일을 사용하여 덤프를 남길 수 있으며, 남긴 덤프는 eclipse 프로젝트에서 제공하는 MAT와 같은 툴을 통해서 분석하면 된다.
>