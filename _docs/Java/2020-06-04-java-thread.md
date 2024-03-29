---
title: Java 스레드(Thread)
category: Java
date:   2020-06-04 00:30:59
comments: true
order: 3
---

## 스레드(Thread) 란?
* 하나의 프로세스 내부에서 독립적으로 실행되는 하나의 작업 단위를 말한다.
* 세부적으로는 운영체제에 의해 관리되는 하나의 작업 혹은 태스크를 의미한다.
* 스레드와 태스크(혹은 작업)은 바꾸어 사용해도 무관합니다.

<br/>

* JVM에 의해 하나의 프로세스가 발생하고 main() 안의 실행문 들이 하나의 스레드입니다.
  + 스레드 상태는 JVM에 의해 기록 관리된다.
* main() 이외의 또 다른 스레드를 만들려면 __Thread 클래스를 상속하거나 Runnable 인터페이스를 구현합니다.__
* 다중 스레드 작업 시에는 __각 스레드 끼리 정보를 주고받을 수 있어 처리 과정의 오류를 줄일 수 있습니다.__
* __프로세스끼리는 정보를 주고받을 수 없습니다.__

## 스레드(Thread) 생성자, 메소드

| 생성자 | 내용 |  
|:--------:|:--------|
| Thread() |  |
| Thread(String s) | 스레드 이름 |
| Thread(Runnable r) | 인터페이스 객체 |
| Thread(Runnable r, String s) | 인터페이스 객체와 스레드 이름 |

<br/>

| 메소드 | 내용 |  
|:--------:|:--------|
|static void sleep(long msec) throws Interrupted Exception| msec에 지정된 밀리초 동안 대기| 
|String getName()| 스레드의 이름을 s로 설정 | 
|void setName(String s)| 스레드의 이름을 s로 설정| 
|void start()| 스레드를 시작 run() 메소드 호출| 
|int getPriority()| 스레드의 우선 순위를 반환 | 
|void setpriority(int p)| 스레드의 우선순위를 p값으로 | 
|boolean isAlive()| 스레드가 시작되었고 아직 끝나지 않았으면 true 끝났으면 false 반환 | 
|void join() throws InterruptedException| 스레드가 끝날 때 까지 대기  | 
|void run()| 스레드가 실행할 부분 기술 (오버라이딩 사용)| 
|void suspend()| 스레드가 일시정지 resume()에 의해 다시시작 할 수 있다. | 
|void resume()| 일시 정지된 스레드를 다시 시작. | 
|void yield()| 다른 스레드에게 실행 상태를 양보하고 자신은 준비 상태로 |

## 스레드(Thread) 생성 예제
* 직접 상속 받아 스레드 생성
* Runnable 인터페이스를 구현해서 생성
* Thread 클래스
  + Thread 클래스로 부터 제공되는 __run()메소드 오버라이딩해서 사용__

#### Thread 클래스 이용하기

```java
//== Thread Class ==//
class MyThread extends Thread {
  
  public void run() {
    try {
      for(int i = 0 ; i < 10 ; i++) {
          //스레드 0.5초동안 대기
          Thread.sleep(500);
          System.out.println("Thread : " + i);
      }
    } catch(InterruptedException e) {
      System.out.println(e);
    }
  }
}

//== Main Class ==//
public class Thread1 {

  public static void main(String[] args) {
		MyThread thread1 = new MyThread();
    MyThread thread2 = new MyThread();
    //1.동시에 같은 숫자가(start)
    thread1.start();
    thread2.start();
    
    //2.번갈아가면서 나옴(run)
    thread1.run();
    thread1.run();
	}
}
```

![java-thread_start_run]({{ site.baseurl }}/images/Language/Java/java-thread_start_run.JPG)

#### Runable 클래스 이용하기
* 현재의 클래스가 이미 다른 클래스로부터 상속 받고 있다면 Runnable 인터페이스를 이용하여 스레드를 생성할 수 있습니다.
* Runnable 인터페이스는 JDK 라이브러리 인터페이스이고 run()메소드만 정의되어 있다.

```java
//== Runnable Class ==//
public class MyRunnable implements Runnable {

	@Override
	public void run() {
		try {
			for (int i = 0; i < 10; i++) {
				Thread.sleep(500);
				System.out.println("Thread="+i);
			}
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
}

//== Main Class ==//
public class Thread2 {
	public static void main(String args[]) {
    // Runnable 인터페이스 객체생성
    MyRunnable Obj1 = new MyRunnable();
    MyRunnable Obj2 = new MyRunnable();
    
    // Runnable 객체를 매개변수로 하여 스레드 객체 th생성
    Thread thread1 = new Thread(Obj1);
    Thread thread2 = new Thread(Obj2);
    
    thread1.start();
    thread2.start();

    thread1.run();
    thread2.run();
  }
}
```

* __run()메소드가 종료하면 스레드는 종료된다.__
  + 스레드를 계속 실행시킬려면 run()메소드를 무한루프 속에 실행되어야합니다.
* __한번 종료한 스레드는 다시 시작시킬수 없습니다.__
  + 스레드 객체를 다시 생성해야 합니다.
* __한 스레드에서 다른 스레드를 강제 종료할 수 있습니다.__

## 스레드(Thread) 생성주기

![java-thread_make_cycle]({{ site.baseurl }}/images/Language/Java/java-thread_make_cycle.JPG)

* Runnable 상태: 쓰레드가 실행되기위한 준비 단계
* Running 상태: 스케줄러에 의해 선택된 쓰레드가 실행되는 단계
* Blocked 상태: 쓰레드가 작업을 완수하지 못하고 잠시 작업을 멈추는 단계

## 스레드(Thread) 생명주기
* __New (생성)__
  + 스레드가 생성되었지만 스레드가 아직 실행할 준비가 되지 않았음
* __Runnable (준비상태)__
  + 스레드가 실행되기 위한 준비단계입니다.
  + CPU를 점유하고 있지않으며 실행(Running 상태)을 하기 위해 대기하고 있는 상태입니다.
  + 코딩 상에서 start() 메소드를 호출하면 run() 메소드에 설정된 스레드가 Runnable 상태로 진입합니다.
  + "Ready" 상태라고도 합니다.
* __Running (실행상태)__
  + CPU를 점유하여 실행하고 있는 상태이며 run() 메서드는 JVM만이 호출 가능합니다.
  + Runnable(준비상태)에 있는 여러 스레드 중 우선 순위를 가진 스레드가 결정되면 JVM이 자동으로 run() 메소드를 호출하여 스레드가 Running 상태로 진입합니다.
* __Dead (종료상태)__
  + Running 상태에서 스레드가 모두 실행되고 난 후 완료 상태입니다. "Done" 상태라고도 합니다.
* __Blocked (지연 상태)__
  + CPU를 점유권을 상실한 상태입니다. 
  + 후에 특정 메서드를 실행시켜 Runnable(준비상태)로 전환합니다.
  + wait() 메소드에 의해 Blocked 상태가 된 스레드는 notify() 메소드가 호출되면 Runnable 상태로 갑니다. 
  + sleep(시간) 메소드에 의해 Blocked 상태가 된 스레드는 지정된 시간이 지나면 Runnable 상태로 갑니다.


## 스레드 생명주기 (더 자세한 그림)

![java-thread_make_cycle_detail]({{ site.baseurl }}/images/Language/Java/java-thread_make_cycle_detail.JPG)

[출처](https://raccoonjy.tistory.com/15)

* NEW : 스레드가 생성되었지만 스레드가 아직 실행할 준비가 되지 않았음
* RUNNABLE : 스레드가 실행되고 있거나 실행준비되어 스케쥴링은 기달리는 상태
* WAITING : 다른 스레드가 notify(), notifyAll()을 불러주기 기다리고 있는 상태(동기화)
* TIMED_WAITING : 스레드가 sleep(n) 호출로 인해 n 밀리초동안 잠을 자고 있는 상태
* BLOCK : 스레드가 I/O 작업을 요청하면 자동으로 스레드를 BLOCK 상태로 만든다.
* TERMINATED : 스레드가 종료한 상태


## Question
#### Java Thread에서 Thread클래스와 Runnable의 차이점은 무엇인가??
Thread는 클래스이고 Runnable는 JDK 라이브러리 인터페이스 입니다. 즉, Thread 클래스를 상속받거나 Runnable 인터페이스를 구현하여 Thread를 생성할 수 있습니다.


Runnable 인터페이스는 run()메소드만 정의 되어있습니다.

#### 언제 Runnable 인터페이스를 이용하는가??
스레드를 생성해야하는 상황에서 현재의 클래스가 이미 다른 클래스로부터 상속 받고 있다면 Runnable 인터페이스를 이용하여 스레드를 생성할 수 있습니다.

## References
* [https://raccoonjy.tistory.com/15](https://raccoonjy.tistory.com/15)