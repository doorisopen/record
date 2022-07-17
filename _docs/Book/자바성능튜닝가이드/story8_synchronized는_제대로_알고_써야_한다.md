# story8. synchronized는 제대로 알고 써야 한다
생성일: 2022년 6월 17일 오후 11:13

---

> 웹 기반 시스템을 개발할 때 **스레드를 직접 건드리면 서비스의 안전성이 떨어질 수 있어 자제**하는 것이 좋다.
> 

WAS는 **여러 개의 스레드가 동작**하도록 되어있다. 그래서 `synchronized`를 자주 사용하게 된다.

그러나, **무조건 안적적인 것은 아니며, 성능에 영향을 미치는 부분**도 있으니 주의해야한다.

# 프로세스(Process)와 스레드(Thread)

클래스를 하나 수행 혹은 WAS를 기동하면, 서버에서 자바 **프로세스가 하나 생성**된다.

- **하나의 프로세스**에는 **여러 개의 스레드**가 생성된다.
    - 프로세스 - 스레드는 1:多 관계
- 스레드는 **Lightweight Process(LWP)**라고도 말한다.
    - 즉, 가벼운 프로세스
- 프로세스에서 만들어 사용하고 있는 **메모리를 공유**한다.

# 자바와 스레드(Thread)

**스레드의 구현**은 다음 2가지가 있다.

- **Thread 클래스를 상속**받는 방법
    - Thread 클래스는 Runnable 클래스를 구현한 클래스
- **Runnable 인터페이스를 구현**하는 방법
    - Runnable 인터페이스를 구현하면 원하는 기능을 추가할 수 있다.(확장성)
    - 구현 클래스를 수행할 때 별도의 스레드 객체를 생성해야한다.

```java
// 1. Thread 클래스 상속
public class ThreadExtends extends Thread {
	public void run() {
		System.out.println("ThreadExtends.run() call");
	}
}

// 2. Runnable 인터페이스 구현
public class RunnableImpl implements Runnable {
	public void run() {
		System.out.println("RunnableImpl.run() call");
	}
}
```

Thread 구현 클래스 실행

```java
public class Main {
	public static void main(String[] args) {
		//RunnableImpl Start
		RunnableImpl runnableImpl = new RunnableImpl();
		Thread threadA = new Thread(runnableImpl); // thread 생성
		threadA.start();

		//ThreadExtends Start
		ThreadExtends threadExtends = new ThreadExtends();		
		threadExtends.start();
	}
}
```

Thread 구현 클래스 실행 코드를 보면 Runnable 인터페이스를 구현하여 생성한 클래스를 수행할 경우 **별도 스레드를 생성하여 수행**하는 것을 볼 수있다.

반면 Thread 클래스를 상속하여 구현한 클래스의 경우 **별도 스레드를 생성하지 않고 수행**하고 있다.(Thread 클래스는 내부에서 Runnable 인터페이스를 구현하고 있기 때문이다)

참고로 스레드를 호출하면서 **우선순위를 따로 지정하지 않아** Thread 클래스를 상속한 클래스가 먼저 수행될 수 도 Runnable 인터페이스를 구현한 클래스가 먼저 수행될 수 도 있다.

## 스레드 관련 메소드(Method)

현재 진행 중인 **스레드를 대기**하도록 하는 메소드 3가지 sleep(), wait(), join()가 있다.

참고로 위 3가지 메소드는 모두 **예외(Exception)**을 던지도록 되어있어 사용 시 반드시 **예외처리가 필요**하다.

- `sleep()`: 명시된 시간 만큼 해당 스레드를 대기시킨다.
    - `static sleep(long millis)`: 명시된 ms 만큼 해당 스레드가 대기한다.
    - `static sleep(long millis, int nanos)`: 명시된 ms + ns(0 ~ 999999) 만큼 해당 스레드가 대기한다.
        - static 메소드이기 때문에 반드시 스레드 객체를 통하지 않아도 사용할 수 있다.
- `wait()`: 모든 클래스의 부모 클래스인 **Object 클래스에 선언**되어 있으므로 어떤 클래스에서도 사용할 수 있다.
    - 명시된 시간 만큼 해당 스레드를 대기시킨다.
    - **sleep() 메소드와 다른점,** 매개변수를 지정하지 않으면 notify()메소드 혹은 notifyAll()메소드가 호출될 때까지 대기한다.
    - 대기 시간 설정은 sleep() 메소드와 동일
- `join()`: 명시된 시간 만큼 해당 스레드가 죽기를 기다린다.
    - 아무런 매개변수를 지정하지 않으면 죽을 때까지 계속 대기한다.

위에 **스레드를 대기하는 것을 ‘모두’ 멈추게** 하는 메소드가 있다.

- `interrupt()`: 대기 하고 있는 스레드들을 중단
    - 중지된 스레드에는 InterruptedException 발생
    - `interrupted()`: 스레드의 상태를 변경
    - `isInterrupted()`: 스레드의 상태만 리턴
- `isAlive()`: 스레드가 살아있는지 확인
- `notify()`, `notifyAll()`: wait 메소드를 멈추기 위한 메소드
    - Object 클래스에 정의되어 있다.
    - notify() 메소드는 객체의 **모니터**와 관련있는 단일 스레드를 깨운다.
    - notifyAll() 메소드는 객체의 **모니터**와 관련있는 모든 스레드를 깨운다.

- [참고] 스레드와 monitor: [https://bgpark.tistory.com/161](https://bgpark.tistory.com/161)

## Interrupt() 메소드는 절대적인 것이 아니다.

interrupt() 메서드를 호출하여 **특정 메소드를 중지시키려고 할 때 항상 해당 메소드가 멈추는 것은 아니다.**

아래는 interrupt()의 설명을 번역한 내용입니다.

- 스레드가 wait(), join(), sleep()에 의해 block이 된 경우, **인터럽트 상태는 지워지고 InterruptedException이 발생**한다.
- 스레드가 인터럽트 가능한 채널의 I/O 작업에 의해 block이 된 경우, **채널이 닫히고 스레드는 인터럽트 상태로 설정**됩니다. 그리고 ClosedByInterruptException이 발생합니다.
- 스레드가 Selector에 의해 block이 된 경우, **스레드는 인터럽트 상태로 설정되고 selection 작업으로 부터 즉시 0이 아닌 양수를 반환**받습니다.(마치 selector가 wakeup 메소드를 호출한 것 처럼)
- 이전 상태가 고정된 것이 없다면 스레드의 상태는 인터럽트 상태로 설정됩니다.

쉽게 말해, **interrupt()는 해당 스레드가 ‘Block’되거나 특정 상태에서만 작동**한다는 말입니다.

# synchronized

> synchronized(동사): 동시에 일어나다. 동시에 진행하다.
> 

`**synchronized**`는 **하나의 객체에 여러 객체가 동시에 접근**하여 처리하는 상황이 발생할 때 사용한다.

**하나의 객체에 여러 요청이 동시에** 들어오면 원하는 처리를 하지 못하고 이상한 결과를 받을 수 있다. 그래서 `**synchronized**` 키워드를 사용해서 **동기화**를 하게 된다.

쉽게 말해 **상태를 가진 객체의 메소드(or 블록)에 synchronized를 사용**하면, 여러 스레드가 해당 객체에 동시에 방문해도 키워드가 선언된 메소드 or 블록에서 “천천히 한 명씩” 제어할 수 있게 된다.

## [참고] 동기화를 고려해야하는 이유

Java는 크게 3가지 **`static`, `stack`, `heap` 영역의 메모리 영역**을 가지고 있습니다.

- `static`: Java 클래스 파일은 필드, 생성자, 메소드로 구성되어있습니다. 그 중 필드 영역에 선언된 변수(전역 변수)와 정적 멤버 변수(static 키워드가 붙은 변수)를 관리하는 영역입니다.
- `stack`: 스레드마다 할당 받는 영역으로 메소드 내에서 정의하는 기본형(primitive type) 지역 변수와 메소드들의 콜스택을 저장하는 영역입니다.
- `heap`: 참조형 데이터(String, Object, …) 타입을 갖는 객체(인스턴스), 배열 등을 저장하는 영역입니다.

자바 멀티 스레드 환경에서는 **스레드들끼리 static 영역과 heap 영역을 공유**합니다. 

그래서 **공유 되는 자원에 대한 동기화 문제를 고려**해야합니다.

---

```java
// ex1) 메소드 synchronized
public synchronized void setGrade(String grade) {
	...
}

// ex2) 블록 synchronized
public void setGrade(String grade) {
	synchronized(grade) {
		...
	}
}
```

위와 같이 메소드, 특정 블록에 synchronized 키워드로 동기화를 할 수 있다.

그럼 어떤 경우에 동기화를 해야할까?

- **하나의 객체를 여러 스레드에서 동시에 사용**할 경우
- **static으로 선언한 객체를 여러 스레드에서 동시에 사용**할 경우

## 하나의 객체에 동시 접근

하나의 객체에 동시에 접근하는 상황을 코드로 구현해 보자

요구사항은 다음과 같다.

- 기부금을 모금하고 처리하는 단체(Contribution)와 기부자(Contributor)가 있다.
- 기부자는 기부 단체에 기부(donate())를 할 수 있다.
    - 한번 기부할 때마다 1원씩 1000번 1000원을 기부한다.
- 기부자는 지금까지 자신이 기부한 기부 금액을 확인할 수 있다.

**기부자(Contributor.class)**

```java
public class Contributor extends Thread { // Thread 상속
    private final String name;
    private final Contribution contribution;

    public Contributor(final String name, final Contribution contribution) {
        this.name = name;
        this.contribution = contribution;
    }

    @Override
    public void run() {
        for (int count = 0; count < 1000; count++) {
            contribution.donate(this, 1L);
        }
        System.out.printf("[%s] donate = %d\n", name, contribution.getTotalDonateMoney());    
		}
}
```

**기부 단체(Contribution.class)**

```java
import java.util.HashMap;
import java.util.Map;

public class Contribution {
    private Long totalDonateMoney;

    public Contribution() {
        this.totalDonateMoney = 0L;
    }

    public void donate(Contributor contributor, Long donateMoney) {
        totalDonateMoney += donateMoney;
				...
    }

    public Long getTotalDonateMoney() {
        return totalDonateMoney;
    }
}
```

**실행 결과(테스트 코드)**

![story8-1](/images/Book/자바성능튜닝가이드/story8-1.png)

테스트 코드는 **기부자가 기부를 하고 기부 단체에 기부 결과를 확인**하는 코드이다.

그러나 10명의 기부자가 천원씩 기부를 하고 최종 결과가 10000원이 출력되어야 하지만 예상과 다른 내용임을 확인 할 수 있다.

이런 결과가 나오는 이유는 여러 스레드가 하나의 Contribution 객체에 접근하도록 되어있기 때문이다. 이 오류를 해결하기 위해서 `synchronized` 키워드를 선언하면 된다.

```java
public class Contribution {
    ...생략

    public synchronized void donate(Contributor contributor, Long donateMoney) {
        totalDonateMoney += donateMoney;
    }
		...
}
```

다시 실행해보면 최종적으로 10000이 찍히는 결과를 확인할 수 있다.(각 스레드는 독립적으로 수행되기 때문에 순서의 차이가 있습니다. 최종 결과가 중요)

![story8-2](/images/Book/자바성능튜닝가이드/story8-2.png)

## [참고] Intrinstic Lock과 synchronized 블록

> **자바의 모든 객체는 락(Lock)을 가진다**고 합니다. 모든 객체가 갖고 있으니 `**고유락(Intrinstic Lock)**`이라고도 하는데, **synchronized 블록이 이 고유락을 사용해서 락을 다룹니다.**
> 

하나의 객체에 동시에 접근해야하는 경우가 있다면 synchronized 키워드를 사용하여 동기화 하면 된다는 것을 알게 되었습니다. 그러나 **synchronized를 어디에(특정 메소드 or 블록) 사용하는가에 따라 Lock의 범위**가 달라집니다.

우리가 위에서 본 예제는 메소드에 synchronized 를 선언해서 동기화한 것 이지만 **정확히 어떤 단위로 락을 가지고 가는지 확인이 어렵**습니다.

위 예시를 재구성하면

```java
// Contributor.class
public class Contributor extends Thread {
    ...

    @Override
    public void run() {
				// 변경 사항
        System.out.printf("[%s] start donate %s \n", getName(), LocalDateTime.now());
        contribution.donate1(this, 1000L);
        System.out.printf("[%s] end donate %s\n", getName(), LocalDateTime.now());
    }
		...
}

// Contribution.class
public class Contribution {

    public synchronized void donate1(Contributor contributor, Long donateMoney) {
				System.out.printf("[%s] contribution donate1 call %s\n", contributor.getName(), LocalDateTime.now());
        donate(donateMoney);
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private void donate(Long donateMoney) {
        totalDonateMoney += donateMoney;
    }
...
}
```

주요하게 변경된 부분은 Contribution 클래스에서 기부(donate) 이후 **5초간 중지 후 종료**하게 되는 부분입니다.

![story8-3](/images/Book/자바성능튜닝가이드/story8-3.png)

위 코드를 실행하면 앞서 본 예제와 동일하게 synchronized 선언으로 **특정 스레드에서 수행하고 있는 동안 다른 스레드에서 접근하지 못함**을 확인할 수 있습니다.

다시 살짝 코드를 변경하여..

```java
// Contributor.class
public class Contributor extends Thread {
    ...

    @Override
    public void run() {
				// 변경 사항
        System.out.printf("[%s] start donate %s \n", getName(), LocalDateTime.now());
        if ("contributor1".equals(name)) {
            contribution.donate1(this, 1000L);
        } else {
            contribution.donate2(this, 1000L);
        }
        System.out.printf("[%s] end donate %s\n", getName(), LocalDateTime.now());
    }
		...
}

// Contribution.class
public class Contribution {

    public synchronized void donate1(Contributor contributor, Long donateMoney) {
				System.out.printf("[%s] contribution donate1 call %s\n", contributor.getName(), LocalDateTime.now());
        donate(donateMoney);
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

		public synchronized void donate2(Contributor contributor, Long donateMoney) {
				System.out.printf("[%s] contribution donate2 call %s\n", contributor.getName(), LocalDateTime.now());
        donate(donateMoney);
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
		...
}
```

Contribution 클래스에는 donate1()과 동일한 donate2()메소드를 추가하였습니다. 그리고 Contributor 클래스는 이름(name)이 contributor1인 경우 donate1()을 그렇지 않은 경우 donate2()를 호출하도록 변경하였습니다.

결과를 보면

![story8-4](/images/Book/자바성능튜닝가이드/story8-4.png)

각 스레드가 서로 다른 메소드를 호출함에도 앞서본 결과와 동일한 것을 확인할 수 있습니다. 

결론은 synchronized를 메소드에 선언하게 되면, **Lock은 개별 메소드에 걸리는 것이 아니라 인스턴스에 Lock이 걸리고 스레드가 그 인스턴스의 Lock을 가지고 있다고 볼 수 있습니다.**

만약 아래와 같이 synchronized를 메소드 내부 블록에 선언하게 되면 인스턴스가 아닌 해당 블록에만 Lock이 걸리게 됩니다.

```java
public void donate1(Contributor contributor, Long donateMoney) {
    synchronized (contributor) {
        System.out.printf("[%s] contribution donate1 call %s\n", contributor.getName(), LocalDateTime.now());
        donate(contributor, donateMoney);
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

추가로 **고유락 재진입**이라는 특성을 통해서도 ‘객체 단위 Lock’을 다시 확인할 수 있습니다.

```java
public class Contribution {
	public synchronized void syncA() {
		System.out.println("call syncA");

		syncB();
	}

	public synchronized void syncB() {
		System.out.println("call syncB");
	}
}

public static void main(String[] args) {
	Contribution contirbution =	new Contribution()
	contribution.syncA();
}
```

syncA를 호출하는 시점에 Contribution 인스턴스의 lock을 획득했기 때문에 synchronized가 선언되어있어도 syncB메소드를 호출할 수 있습니다. 

만약 고유락 재진입 특성이 없다면 syncA와 syncB가 서로 상호 호출하거나 공유 자원에 대한 선점이 있으면 데드락이 발생할 가능성이 높아지게 됩니다.

## **static으로 선언한 객체 동시에 사용**

만약 필드 영역에 선언된 totalDonateMoney를 static으로 선언하고 수행하면 다음과 같다.

```java
public class Contribution {
    private static Long totalDonateMoney = 0L;
		...
		
		public void donate1(Contributor contributor, Long donateMoney) {
			...
		}
}
```

![story8-5](/images/Book/자바성능튜닝가이드/story8-5.png)

결과를 보니 예상하지 못한 결과가 출력되고 있다. 

**필드 영역의 변수에 static 을 선언하게 되면 객체(인스턴스)의 변수가 아닌 클래스 변수가 된다. 즉,  static 영역에서 관리되는 변수로 모든 스레드가 접근할 수 있는 정보**이기 때문에 사용 시 주의가 필요하다.

위 문제를 해결하기 위해서는 donate()에 static, synchronized 키워드 선언해줘야 해결할 수 있다.

```java
public static synchronized void donate1(Contributor contributor, Long donateMoney) {
}

private static void donate(Long donateMoney) {
}
```

이유는 synchronized는 각각의 객체에 대한 동기화를 하는 것이기 때문에, totalDonateMoney 에 대한 동기화는 되지 않는다. 때문에 메서드도 클래스 메서드로 참조하도록 static을 선언해줘야 한다.

![story8-6](/images/Book/자바성능튜닝가이드/story8-6.png)

참고로 static 메소드에 synchronized 블록이 있으면 **class 단위로 락**을 걸게 됩니다. 

```java
// Contribution.class
public class Contribution {

    public static synchronized void donate1(Contributor contributor, Long donateMoney) {
        System.out.printf("[%s] contribution donate1 call %s\n", contributor.getName(), LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public synchronized void donate2(Contributor contributor, Long donateMoney) {
        System.out.printf("[%s] contribution donate2 call %s\n", contributor.getName(), LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
		...
}
```

![story8-7](/images/Book/자바성능튜닝가이드/story8-7.png)

결과를 보면 각 스레드가 서로 다른 메소드를 호출한 경우, 인스턴스에 lock을 잡고 있는게 아닌 개별로 동작하고 있는 것을 확인할 수 있습니다.

따라서 위와 같이 혼용해서 사용할 땐 주의가 필요합니다.

# 자바에서 제공하는 동기화

JDK 5.0에서 추가된 java.util.concurrent 패키지는 동기화 관련 여러 API를 제공합니다.

주요 용어를 정리하면

- `Lock`: 실행 중인 스레드를 간단한 방법으로 정지 시켰다가 실행시킨다. 상호 참조로 인해 발생하는 데드락을 피할 수 있다.
- `Executors`: 스레드를 더 효율적으로 관리할 수 있는 클래스들을 제공한다. 스레드 풀도 제공하므로, 필요에 따라 유용하게 사용할 수 있다.
- `Concurrent 콜렉션`: 앞서 살펴본 콜렉션의 클래스들을 제공한다.
- `Atomic 변수`: 동기화가 되어 있는 변수를 제공한다. 이 변수를 사용하면, synchronized 식별자를 메소드에 지정할 필요 없이 사용할 수 있다.

# JVM의 synchronization 동작

자바의 HotSpot VM은 `자바 모니터(monitor)`를 제공함으로써 스레드들이 `상호 배제 프로토콜(mutual exclusion protocol)`에 참여할 수 있도록 돕는다.

자바 모니터는 잠긴 상태(lock)나 풀림(unlocked) 중 하나이며, **동일한 모니터에 진입한 여러 스레드들 중에서 한 시점에는 단 하나의 스레드만 모니터를 가질 수 있다.** 

다시 말하면, 모니터를 가진 스레드만 모니터에 의해서 **보호되는 영역**에 들어가서 작업을 할 수 있다. 여기서 **보호된 영역이란 synchronized 로 감싸진 블록들을 의미**한다.

모니터를 보유한 영역에서의 작업을 마치면, 모니터는 다른 대기중인 스레드에게 넘어간다.

# References

- ****Intrinsic Locks and Synchronization:**** [https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/java-intrinsic-locks.html](https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/java-intrinsic-locks.html)
- java 동기화 및 lock [https://enumclass.tistory.com/169](https://enumclass.tistory.com/169)