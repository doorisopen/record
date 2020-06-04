---
title: 가비지 컬렉션(GC, Garbage Collection)
category: Java
comments: true
order: 5
---

# 목차
> 1. __가비지 컬렉션(GC, Garbage Collection) 이란?__
> 2. __Young 영역의 구성__
> 3. __Old 영역에 대한 GC__

`C 나 C++` 에서는 OS 레벨의 메모리에 직접 접근하기 때문에 free() 라는 메소드를 호출하여 할당받았던 메모리를 명시적으로 해제해주어야 한다. 그렇지 않으면 `memory leak` 이 발생하게 되고, 현재 실행중인 프로그램에서 memory leak 이 발생하면 다른 프로그램에도 영향을 끼칠 수 있다.

반면, `Java`는 OS 의 메모리 영역에 직접적으로 접근하지 않고 `JVM(Java Virtual Machine)` 이라는 가상머신을 이용해서 간접적으로 접근한다. JVM 은 C 로 쓰여진 또 다른 프로그램인데, __오브젝트가 필요해지지 않는 시점에서 알아서 free() 를 수행하여 메모리를 확보한다.__ 웹 애플리케이션을 만들 때 모든 것을 다 직접 개발하여 쓰기보다 검증된 라이브러리나 프레임워크를 이용하는 것이 더 안전하고 편리한 것 처럼, 메모리 관리라는 까다로운 부분을 자바 가상머신에 모두 맡겨버리는 것이다.

프로그램 실행시 JVM 옵션을 주어서 OS에 요청한 사이즈 만큼의 메모리를 할당 받아서 실행하게된다. 할당받은 이상의 메모리를 사용하게 되면 에러가 나면서 자동으로 프로그램이 종료된다. 그러므로 현재 프로세스에서 메모리 누수가 발생하더라도 현재 실행중인 것만 죽고, 다른 것에는 영향을 주지 않는다.

이렇게 Java는 가상머신을 사용함으로써 (운영체제로 부터 독립적이라는 장점 외에도) OS 레벨에서의 memory leak 은 불가능하게 된다는 장점이 있다.

자바가 메모리 누수현상을 방지하는 또 다른 방법이 `가비지 컬렉션`이다.

# 가비지 컬렉션 (Garbage Collection)
프로그래머는 힙을 사용할 수 있는 만큼 자유롭게 사용하고, 더 이상 사용되지 않는 오브젝트들은 `가비지 컬렉션`을 담당하는 __프로세스가 자동으로 메모리에서 제거하도록 하는 것이 가비지 컬렉션의 기본 개념__ 이다.

자바는 `가비지 컬렉션`에 아주 단순한 규칙을 적용한다.

> Heap 영역의 오브젝트 중 stack 에서 도달 불가능한 (Unreachable) 오브젝트들은 가비지 컬렉션의 대상이 된다.

## 가비지 컬렉션 예시

```
public class Main {
    public static void main(String[] args) {
        String URL = "https://";
        URL += "doorisopen.github.io";
        System.out.println(URL);
    }
}
```
위 코드에서 `String URL = "https://";` 구문이 실행된 뒤 스택과 힙은 아래와 같다.

![java-gc1](/images/Language/Java/java-gc1.JPG)


다음 구문인 `URL += "doorisopen.github.io";` 문자열 더하기 연산이 수행되는 과정에서(String은 불변객체이므로) 기존에 있던 `"https://"` 스트링에 `"doorisopen.github.io"` 를 덧붙이는 것이 아니라, 문자열에 대한 더하기 연산이 수행된 결과가 새롭게 heap 영역에 할당된다. 그 결과를 그림으로 표현하면 아래와 같다.

![java-gc2](/images/Language/Java/java-gc2.JPG)


__Stack 에는 새로운 변수가 할당되지 않는다.__ 문자열 더하기 연산의 결과인 `"https://doorisopen.github.io"` 가 새롭게 heap 영역에 생성되고, 기존에 `"https://"` 를 레퍼런스 하고 있던 `URL` 변수는 새롭게 생성된 문자열을 레퍼런스 하게 된다.

> 기존의 "https://" 라는 문자열을 레퍼런스 하고 있는 변수는 아무것도 없으므로 __Unreachable 오브젝트__ 가 된다.

JVM 의 Garbage Collector 는 Unreachable Object 를 우선적으로 메모리에서 제거하여 메모리 공간을 확보한다. Unreachable Object 란 Stack 에서 도달할 수 없는 Heap 영역의 객체를 말하는데, 지금의 예제에서 "https://" 문자열과 같은 경우가 되겠다. 아주 간단하게 이야기해서 이런 경우에 Garbage Collection 이 일어나면 Unreachable 오브젝트들은 메모리에서 제거된다.

Garbage Collection 과정은 Mark and Sweep 이라고도 한다. JVM의 Garbage Collector 가 스택의 모든 변수를 스캔하면서 각각 어떤 오브젝트를 레퍼런스 하고 있는지 찾는과정이 Mark 다. Reachable 오브젝트가 레퍼런스하고 있는 오브젝트 또한 marking 한다. 첫번째 단계인 marking 작업을 위해 모든 스레드는 중단되는데 이를 stop the world 라고 부르기도 한다. (System.gc() 를 생각없이 호출하면 안되는 이유이기도 하다)

그리고 나서 mark 되어있지 않은 모든 오브젝트들을 힙에서 제거하는 과정이 Sweep 이다.

Garbage Collection 이라고 하면 garbage 들을 수집할 것 같지만 실제로는 garbage 를 수집하여 제거하는 것이 아니라, garbage 가 아닌 것을 따로 mark 하고 그 외의 것은 모두 지우는 것이다. 만약 힙에 garbage 만 가득하다면 제거 과정은 즉각적으로 이루어진다.

Garbage Collection 이 일어난 후의 메모리 상태는 아래 사진의 __Heap 영역에서 1)이 제거된 상태와 같을 것 이다.__

![java-gc2](/images/Language/Java/java-gc2.JPG)


## 가비지 컬렉션 과정 (Generational Garbage Collection)
`가비지 컬렉션 과정`에 대해서 알아보기 전에 알아야 할 용어가 있다. 바로 `stop-the-world`이다.

### stop-the-world 란?
`GC`을 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다. `stop-the-world`가 발생하면 GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다. 어떤 GC 알고리즘을 사용하더라도 stop-the-world는 발생한다. 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것이다.

__Java는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않는다.__ 가끔 __명시적으로 해제하려고 해당 객체를 null로 지정하거나 System.gc() 메서드를 호출한다.__ 그러나 null로 지정하는 것은 큰 문제가 안 되지만, __System.gc() 메서드를 호출하는 것은 시스템의 성능에 매우 큰 영향을 끼치__ 므로 System.gc() 메서드는 절대로 사용하면 안 된다

`가비지 컬렉터`는 두 가지 전제 조건 하에 만들어졌다.

* 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
* 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

이러한 가설을 __'weak generational hypothesis'__ 라 한다. 이 가설의 장점을 최대한 살리기 위해서 `HotSpot VM`에서는 크게 2개로 물리적 공간을 나누었다. 둘로 나눈 공간이 `Young 영역`과 `Old 영역`이다.

### * Young 영역(Yong Generation 영역)
새롭게 생성한 객체의 대부분이 여기에 위치한다. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다. 이 영역에서 객체가 사라질때 `Minor GC`가 발생한다고 말한다.

### * Old 영역(Old Generation 영역)
접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 이 영역에서 객체가 사라질 때 `Major GC(혹은 Full GC)`가 발생한다고 말한다. 

![java-gc3](/images/Language/Java/java-gc3.JPG)


### * Permanent Generation 영역(이하 Perm 영역)
위 그림의 `Permanent Generation 영역(이하 Perm 영역)`은 Method Area라고도 한다. __객체나 억류(intern)된 문자열 정보를 저장하는 곳__ 이며, Old 영역에서 살아남은 객체가 영원히 남아 있는 곳은 절대 아니다. 이 영역에서 GC가 발생할 수도 있는데, 여기서 GC가 발생해도 Major GC의 횟수에 포함된다.

### Old 영역 객체가 Young 영역 객체를 참조하는 경우
이러한 경우를 처리하기 위해서 Old 영역에는 512바이트의 덩어리(chunk)로 되어 있는 `카드 테이블(card table)`이 존재한다.

`카드 테이블`에는 Old 영역에 있는 객체가 __Young 영역의 객체를 참조할 때마다 정보가 표시된다.__ Young 영역의 GC를 실행할 때에는 Old 영역에 있는 모든 객체의 참조를 확인하지 않고, __이 카드 테이블만 뒤져서 GC 대상인지 식별한다.__

![java-gc4](/images/Language/Java/java-gc4.JPG)


카드 테이블은 `write barrier`를 사용하여 관리한다. __write barrier는 Minor GC를 빠르게 할 수 있도록 하는 장치이다.__ write barrirer때문에 약간의 오버헤드는 발생하지만 전반적인 GC 시간은 줄어들게 된다.

### 가비지 컬렉터(gabage collector)
`데몬 스레드`를 이용하는 가장 대표적인 예로 `가비지 컬렉터(gabage collector)`를 들 수 있다.

`가비지 컬렉터(gabage collector)`란 프로그래머가 동적으로 할당한 메모리 중 더 이상 사용하지 않는 영역을 자동으로 찾아내어 해제해 주는 __데몬 스레드__ 이다.

자바에서는 프로그래머가 메모리에 직접 접근하지 못하게 하는 대신에 `가비지 컬렉터`가 자동으로 메모리를 관리해 준다.

이러한 `가비지 컬렉터`를 이용하면 프로그래밍을 하기가 훨씬 쉬워지며, 메모리에 관련된 버그가 발생할 확률도 낮아진다.

 
보통 `가비지 컬렉터`가 동작하는 동안에는 프로세서가 일시적으로 중지되므로, 필연적으로 성능의 저하가 발생한다.

하지만 요즘에는 가비지 컬렉터의 성능이 많이 향상되어, 새롭게 만들어지는 대부분의 프로그래밍 언어에서 `가비지 컬렉터`를 제공하고 있다. 


# Young 영역의 구성
Young 영역은 3개의 영역으로 나뉜다.

* Eden 영역
* Survivor 영역(2개)

![java-gc5](/images/Language/Java/java-gc5.JPG)


Survivor 영역이 2개이기 때문에 총 3개의 영역으로 나뉘는 것이다. 각 영역의 처리 절차를 순서에 따라서 기술하면 다음과 같다.

1. 새로운 오브젝트는 Eden 영역에 할당된다. 두개의 Survivor Space 는 비워진 상태로 시작한다.
2. Eden 영역이 가득차면, MinorGC 가 발생한다.
3. Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다.
4. 다음 MinorGC 가 발생할때, Eden 영역에는 3번과 같은 과정이 발생한다. Unreachable 오브젝트들은 지워지고, Reachable 오브젝트들은 Survivor Space 로 이동한다. 기존에 S0 에 있었던 Reachable 오브젝트들은 S1 으로 옮겨지는데, 이때, age 값이 증가되어 옮겨진다. 살아남은 모든 오브젝트들이 S1 으로 모두 옮겨지면, S0 와 Eden 은 클리어 된다. Survivor Space 에서 Survivor Space 로의 이동은 이동할때마다 age 값이 증가한다.
5. 다음 MinorGC 가 발생하면, 4번 과정이 반복되는데, S1 이 가득차 있었으므로 S1 에서 살아남은 오브젝트들은 S0 로 옮겨지면서 Eden 과 S1 은 클리어 된다. 이때에도, age 값이 증가되어 옮겨진다. Survivor Space 에서 Survivor Space 로의 이동은 이동할때마다 age 값이 증가한다.
6. Young Generation 에서 계속해서 살아남으며 age 값이 증가하는 오브젝트들은 age 값이 특정값 이상이 되면 Old Generation 으로 옮겨지는데 이 단계를 Promotion 이라고 한다.
7. MinorGC 가 계속해서 반복되면, Promotion 작업도 꾸준히 발생하게 된다.
8. Promotion 작업이 계속해서 반복되면서 Old Generation 이 가득차게 되면 MajorGC 가 발생하게 된다.

이 절차를 보면 알겠지만 __Survivor 영역 중 하나는 반드시 비어 있는 상태로 남아 있어야 한다.__ 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 여러분의 시스템은 정상적인 상황이 아니라고 생각하면 된다.

### HotSpot VM의 빠른 메모리 할당(참고)
HotSpot VM에서는 보다 __빠른 메모리 할당을 위해서 두 가지 기술을 사용한다.__ 하나는 `bump-the-pointer`라는 기술이며, 다른 하나는 `TLABs(Thread-Local Allocation Buffers)`라는 기술이다.

`bump-the-pointer`는 __Eden 영역에 할당된 마지막 객체를 추적한다.__ 마지막 객체는 Eden 영역의 맨 위(top)에 있다. 그리고 그 다음에 생성되는 객체가 있으면, 해당 객체의 크기가 Eden 영역에 넣기 적당한지만 확인한다. 만약 해당 객체의 크기가 적당하다고 판정되면 Eden 영역에 넣게 되고, 새로 생성된 객체가 맨 위에 있게 된다. 따라서, 새로운 객체를 생성할 때 마지막에 추가된 객체만 점검하면 되므로 매우 빠르게 메모리 할당이 이루어진다.

그러나 멀티 스레드 환경을 고려하면 이야기가 달라진다. Thread-Safe하기 위해서 만약 여러 스레드에서 사용하는 객체를 Eden 영역에 저장하려면 락(lock)이 발생할 수 밖에 없고, lock-contention 때문에 성능은 매우 떨어지게 될 것이다. HotSpot VM에서 이를 해결한 것이 TLABs이다.

각각의 스레드가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가질 수 있도록 하는 것이다. 각 쓰레드에는 자기가 갖고 있는 TLAB에만 접근할 수 있기 때문에, bump-the-pointer라는 기술을 사용하더라도 아무런 락이 없이 메모리 할당이 가능하다.

> 위에서 이야기한 두 가지 기술(bump-the-pointer, TLABs)을 반드시 기억하고 있을 필요는 없다. 그러나 __Eden 영역에 최초로 객체가 만들어지고, Survivor 영역을 통해서 Old 영역으로 오래 살아남은 객체가 이동한다는 사실은 꼭 기억하기 바란다.__

# Old 영역에 대한 GC
__Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다.__ GC 방식에 따라서 처리 절차가 달라지므로, 어떤 GC 방식이 있는지 살펴보면 이해가 쉬울 것이다. GC 방식은 JDK 7을 기준으로 5가지 방식이 있다.

* Serial GC
* Parallel GC
* Parallel Old GC(Parallel Compacting GC)
* Concurrent Mark & Sweep GC(이하 CMS)
* G1(Garbage First) GC

이 중에서 운영 서버에서 절대 사용하면 안 되는 방식이 __Serial GC__ 다. Serial GC는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식이다. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어진다.

### Serial GC (-XX:+UseSerialGC)
Young 영역에서의 GC는 앞 절에서 설명한 방식을 사용한다. Old 영역의 GC는 mark-sweep-compact이라는 알고리즘을 사용한다. 이 알고리즘의 첫 단계는 Old 영역에 살아 있는 객체를 식별(Mark)하는 것이다. 그 다음에는 힙(heap)의 앞 부분부터 확인하여 살아 있는 것만 남긴다(Sweep). 마지막 단계에서는 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다(Compaction).

Serial GC는 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식이다.

### Parallel GC (-XX:+UseParallelGC)
Parallel GC는 Serial GC와 기본적인 알고리즘은 같지다. 그러나 Serial GC는 GC를 처리하는 스레드가 하나인 것에 비해, Parallel GC는 GC를 처리하는 쓰레드가 여러 개이다. 그렇기 때문에 Serial GC보다 빠른게 객체를 처리할 수 있다. Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 유리하다. Parallel GC는 Throughput GC라고도 부른다.

### Parallel Old GC(-XX:+UseParallelOldGC)
Parallel Old GC는 JDK 5 update 6부터 제공한 GC 방식이다. 앞서 설명한 Parallel GC와 비교하여 Old 영역의 GC 알고리즘만 다르다. 이 방식은 Mark-Summary-Compaction 단계를 거친다. Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르며, 약간 더 복잡한 단계를 거친다.

### CMS GC (-XX:+UseConcMarkSweepGC)
다음 그림은 Serial GC와 CMS GC의 절차를 비교한 그림이다. 그림에서 보듯이 CMS GC는 지금까지 설명한 GC 방식보다 더 복잡하다.

초기 Initial Mark 단계에서는 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝낸다. 따라서, 멈추는 시간은 매우 짧다. 그리고 Concurrent Mark 단계에서는 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인한다. 이 단계의 특징은 다른 스레드가 실행 중인 상태에서 동시에 진행된다는 것이다.

그 다음 Remark 단계에서는 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다. 마지막으로 Concurrent Sweep 단계에서는 쓰레기를 정리하는 작업을 실행한다. 이 작업도 다른 스레드가 실행되고 있는 상황에서 진행한다.

이러한 단계로 진행되는 GC 방식이기 때문에 stop-the-world 시간이 매우 짧다. 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용하며, Low Latency GC라고도 부른다.

그런데 CMS GC는 stop-the-world 시간이 짧다는 장점에 반해 다음과 같은 단점이 존재한다.

* 다른 GC 방식보다 메모리와 CPU를 더 많이 사용한다.
* Compaction 단계가 기본적으로 제공되지 않는다.

따라서, CMS GC를 사용할 때에는 신중히 검토한 후에 사용해야 한다. 그리고 조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 stop-the-world 시간이 더 길기 때문에 Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인해야 한다.

### G1 GC
Garbage First 라는 의미의 G1 가비지 컬렉터는 Java 7 부터 사용가능하며, 장기적으로 CMS 컬렉터를 대체하기 위해 만들어졌다. `-XX:+UseG1GC` 옵션으로 사용가능하다.

G1 GC는 바둑판의 각 영역에 객체를 할당하고 GC를 실행한다. 그러다가, 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행한다. 즉, 지금까지 설명한 Young의 세가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식이라고 이해하면 된다.

G1 GC 에 대한 자세한 내용은 [Oracle Getting Started with the G1 Garbage Collector](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html) 를 참고하기 바란다


<hr/>


<br />
<br />

> _이 게시글은 [Navae D2](https://d2.naver.com/helloworld/1329)의 게시글을 기반으로 `가비지 컬렉션`에 대한 내용을 학습하고 정리한 내용 입니다. (사진 출처 [Navae D2](https://d2.naver.com/helloworld/1329) 외 Reference 입니다)_

<br />
<br />

# Reference
> * [d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)
> * [tcpschool.com/java/java_thread_multi](http://tcpschool.com/java/java_thread_multi)
> * [yaboong.github.io/java-garbage-collection/](https://yaboong.github.io/java/2018/06/09/java-garbage-collection/)
> * [www.oracle.com](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)