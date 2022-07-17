# story17. 도대체 GC는 언제 발생할까?
생성일: 2022년 6월 30일 오전 1:03

---

자바 기반의 시스템을 개발하면서 `**GC(Garbage Collection)**`를 한번쯤 들어봤을 것이다. 그러나, GC가 어떻게 동작하는지에 대해서 잘 모르는 개발자들이 많이 있다. 물론 이런 사실이 중요한 부분은 아니다.

여기서 중요한 점은 유닉스나 리눅스 혹은 윈도우 서버든 **Full GC를 수행하는 시점에는 해당 JVM에서 처리되지 않는다**는 것이다. 다시말해, GC를 많이 하면 할 수록 **응답 시간**에 많은 영향을 끼친다는 것이다.

# GC란?

C언어의 경우 메모리 관리를 개발자가 직접 관리할 수 있는 반면, **자바에서 메모리는 GC라는 알고리즘을 통하여 관리**하기 때문에 개발자가 메모리를 처리하기 위한 로직을 개발할 필요가 없다.(만들어서도 않된다)

자바에서 GC의 대상이 되는 것은 바로 **객체**이다. 하나의 객체는 메모리를 점유하고, 필요하지 않으면 메모리에서 해제되어야 한다.

```java
// a라는 객체가 메모리를 점유한다
String a = new String();

// GC 대상이 되는 경우
String pre = "hello";
String suf = "world";
String word = pre + suf; // pre, suf 객체는 더 이상 필요가 없는 객체 즉, GC의 대상이 된다.
```

즉, **더 이상 필요가 없는 객체를 효과적으로 처리하는 작업을 GC라고 한다.**

# Java Runtime data erea

![story17-1](/images/Book/자바성능튜닝가이드/story17-1.png)

- **Class loader subsystem:** 클래스나 인터페이스를 JVM으로 로딩하는 기능 수행
- **Execution engine:** 로딩된 클래스의 메서드들에 포함되어 있는 모든 인스트럭션 정보를 실행한다.

자바에서 데이터를 처리하기 위한 영역(메모리 영역)은 다음과 같다.

- PC 레지스터
- JVM 스택
- **힙(heap): GC가 발생하는 영역**
- 메서드 영역
- 런타임 상수(constant) 풀
- 네이티브 메서드 스택

## Heap 메모리

- 클래스 인스턴스, 배열이 해당 영역에 쌓인다.
- 해당 영역은 공유(shared) 메모리라고도 불린다.
    - 여러 스레드에서 공유하는 데이터들이 저장되는 메모리이다.

## Non Heap 메모리

자바의 내부 처리를 위해서 필요한 영역으로 주요 영역은 **메서드 영역**이다.

### 메서드 영역

- 모든 JVM 스레드에서 공유
- 저장되는 데이터
    - **런타임 상수 풀:** 자바의 클래스 파일에는 constant pool이라는 정보가 포함되어 있다. 이 constant pool에 대한 정보를 실행 시에 참조하기 위한 영역
        - 실제 **상수 값도 포함**될 수 있다.
        - 실행 시에 **변하게 되는 필드 참조 정보도 포함**
    - 필드 정보에는 메서드 데이터, 메서드와 생성자 코드가 있다.

### JVM 스택

- 스레드가 시작할 때 JVM 스택이 생성
- 메서드가 호출되는 정보인 프레임(frame)이 저장된다.
- 지역 변수와 임시 결과, 메서드 수행과 리턴에 관련된 정보들도 포함된다.

### 네이티브 메서드 스택

- 자바가 아닌 다른 언어(C언어)로 된 코드들이 실행하게 될 때의 스택 정보를 관리한다.

### PC 레지스터

- 자바의 스레드들은 각자의 PC(Program Counter) 레지스터를 갖는다.
- 네이티브한 코드를 제외한 모든 자바 코드들이 수행될 때 **JVM의 인스트럭션 주소를 PC 레지스터에 보관**한다.

![story17-2](/images/Book/자바성능튜닝가이드/story17-2.png)

**힙 영역과 메서드 영역은 JVM이 시작될 때 생성**된다.

# GC의 원리

GC의 역할

- 메모리 할당
- 사용 중인 메모리 인식
- 사용하지 않는 메모리 인식

**사용하지 않는 메모리 인식** 작업을 수행하지 않으면, 할당한 메모리 영역이 꽉 차서 JVM에 **행(Hang)**이 걸리거나 더 많은 메모리를 할당(메모리를 늘리려는?)하려는 현상이 발생한다.

만약 JVM의 최대 메모리 크기를 지정해서 전부 사용한 다음, GC를 해도 더 이상 사용 가능한 메모리 영역이 없는데 계속 메모리를 할당하려고 하면 OutOfMemoryError가 발생하여 JVM이 다운될 수도 있다.

> **행(Hang)이란** 서버가 요청을 처리 못하고 있는 상태를 의미
> 

# 힙 영역

![story17-3](/images/Book/자바성능튜닝가이드/story17-3.png)

힙 영역은 크게 Young, Old 영역으로 나뉜다.(**Perm(Permanent) 영역이 있었는데 JDK 8부터 사라졌다**) 

그리고 Young 영역은 **Eden 영역 및 2개의 Survivor(Survival) 영역**으로 나뉜다.

## [참고] Perm → Metaspace 변경 (JDK 8)

**힙 영역**이 Java 8 이전에는 New, Survival, Old, Perm, Native 였는데

Java 8 부터는 New, Survival, Old, Metaspace로 변경됐다.

**Java 7, 8 주요 정보(Meta Data) 저장 영역 변화**

| 주요 정보 | 저장 영역(Java 7) | 저장 영역(Java 8) |
| --- | --- | --- |
| Class의 Meta 정보(pkg path 정보(text 정보)) | Permanent | Metaspace |
| Method Meta 정보 | Permanent | Metaspace |
| static Object | Permanent | Heap |
| 상수화된 String Object | Permanent | Heap |
| Class와 관련된 배열 객체 Meta 정보 | Permanent | Metaspace |
| JVM 내부적인 객체들과 최적화 컴파일러(JIT)의 최적화 정보 | Permanent | Metaspace |

주요 변경사항은 

- **Perm 영역에 저장되어 문제를 유발하던 static Object는 Heap 영역으로 옮겨서 최대한 GC 대상이 되도록 변경**
    - OutOfMemoryError: PermGen Space error가 발생한 경우가 있는데 이는 PermGen 영역이 꽉 찼을 때와 Memory Leak이 발생했을 때 생기게 된다. Memory Leak의 가장 흔한 이유중 하나는 메모리에 로딩된 클래스와 클래스 로더가 종료될 때 메모리에 올라간 객체들이 GC가 발생하지 않을 때 발생한다.
- **수정될 필요가 없는 정보만 Metaspace에 저장하고 Metaspace는 JVM이 필요에 따라서 리사이징할 수 있는 구조로 개선**
    - Perm 영역은 Java 8부터 Metaspace로 완벽하게 대체
    - Metaspace는 클래스의 메타 데이터를 native 메모리에 저장하고 메모리가 부족할 경우 이를 자동으로 늘려준다.
    - Java 8부터 OutOfMemory Error: PermGen Space error가 발생하지 않는다. 따라서 JVM 옵션으로 사용한 PermSize와 MaxPermSize는 더 이상 사용할 필요가 없어짐
        - 대신 MetaspaceSize 및 MaxMetaspaceSize가 새롭게 생김 이 두 값으로 Metaspace의 기본 값을 변경하고 최대 값을 제한할 수 있다.

## 메모리 할당 과정

메모리에 객체가 생성되면, 아래와 같이 가장 왼쪽인 **Eden 영역에 객체가 지정**된다.

|  | Young 영역 |  | Old |
| --- | --- | --- | --- |
| Eden | Survival1 | Survival2 | 메모리 영역 |

이후 Eden 영역에 데이터가 가득 차면(Minor GC 발생), 이 영역에 있던 객체가 어디론가 옮겨지거나 삭제되어야 하는데 이 때 **옮겨지는 위치가 Survival 영역**이다.

두 개의 Survival 영역 사이에 **우선 순위가 있는 것은 아니고** 두 개의 영역 중 **한 영역은 반드시 비어 있어야 한다.**

Eden 영역에서 GC가 발생하고 살아 남은 객체는 아래와 같이 Survival 영역 중 한 곳에 이동하게 된다.

|  | Young 영역 |  | Old |
| --- | --- | --- | --- |
| Eden | Survival1 | Survival2 | 메모리 영역 |

Survival 영역이 가득 차면, GC가 발생하고 Eden 영역에 있는 객체와 가득 찬 Survival 영역에 있는 객체가 비어있는 Survival 영역으로 이동한다(Survival 간의 이동은 age 값이 증가하게 된다.)

이렇게 계속해서 Eden → Survival → Survival 로 이동하며 age 값이 증가되는데 age 값이 특정 값 이상이 되면 Old 영역으로 옮겨지게 된다.

> Young 영역에서 Old 영역으로 넘어가는 객체 중 **Survival 영역을 거치지 않고 바로 Old 영역으로 이동하는 객체**가 있을 수 있는데, 객체의 크기가 아주 큰 경우 발생할 수 있다. 

만약 Servival 영역의 크기가 16MB라 하고 객체의 크기가 20MB를 점유한다고 할 때, Eden 영역에서 Servival 영역으로 이동할 수 없어 Old 영역으로 이동하게 된다.
> 

참고: [https://doorisopen.github.io/record/Java/2020-02-05-java-garbage-collection](https://doorisopen.github.io/record/Java/2020-02-05-java-garbage-collection)

# GC의 종류

- Minor GC: Young 영역에서 발생하는 GC
- Major GC: Old 영역이나 Perm 영역에서 발생하는 GC

위 2가지 GC가 어떻게 상호 작용하느냐에 따라 GC방식에 차이가 나며, 성능에도 영향을 준다.

GC가 발생하거나 객체가 각 영역에서 다른 영역으로 이동할 때 애플리케이션의 병목이 발생하면서 성능에 영향을 주게 된다. 

그래서 HotSpot JVM에서는 **`스레드 로컬 할당 버퍼(TLABs: Thread-Local Allocation Buffers)`**라는 것을 사용한다.

이를 통해 **각 스레드별 메모리 버퍼를 사용하면 다른 스레드에 영향을 주지 않는 메모리 항당 작업이 가능**해진다.

# GC 방식(5가지)

JDK 7 이상에서 지원하는 GC 방식

- Serial Collector
- Parallel Collector
- Parallel Compacting Collector
- Concurrent Mark-Sweep(CMS) Collector
- Garbage First Collector(G1)
    - JDK 7부터 정식으로 사용

위 GC 방식은 WAS나 자바 애플리케이션 수행 시 옵션을 지정하여 선택한 수 있다.

## Serial Collector

Young 영역과 Old 영역이 연속적으로 처리되며 하나의 CPU를 사용한다.

Sun에서는 이 처리를 수행할 때를 Stop-the-world라고 표현한다. 

다시말해, 콜렉션이 수행될 때 애플리케이션 수행이 정지된다.

![story17-4](/images/Book/자바성능튜닝가이드/story17-4.png)

1. 살아 있는 객체들은 Eden 영역에 있다
2. Eden 영역이 꽉차게 되면 To Survival 영역(비어있는 영역)으로 살아 있는 객체가 이동한다. 
    - 이때 객체의 크기가 너무 크면 Old 영역으로 이동
    - From Survival 영역에 살아있는 객체는 To Survival 영역으로 이동
3. To Survival 영역이 가득 찼을 경우, Eden 영역이나 From Survival 영역에 남아 있는 객체들은 Old 영역으로 이동한다.

Old 영역에 있는 객체들은 Mark-sweep-compact 콜렉션 알고리즘을 따른다.

이 알고리즘은 쓰이지 않는 개체를 표시해서 삭제하고 한 곳으로 모으는 알고리즘이다.

**Mark-sweep-compact 수행절차**

1. Old 영역으로 이동된 객체들 중 살아있는 객체를 식별(mark)
2. Old 영역의 객체들을 훑는 작업을 수행하여 쓰레기 객체를 식별(sweep)
3. 필요 없는 객체들을 지우고 살아 있는 객체들을 한 곳으로 모은다(compact)

**Serial Collector**는 일반적으로 **클라이언트 종류의 장비에서 많이 사용**된다.

다시 말하면, 대기 시간이 많아도 크게 문제되지 않는 시스템에서 사용된다는 의미이다. Serial Collector를 명시적으로 지정하려면 자바 명령 옵션에 `-XX:+UseSerialGC` 를 지정하면 된다.

## Parallel Collector

![story17-5](/images/Book/자바성능튜닝가이드/story17-5.png)

Serial GC vs Parallel GC 차이 (출처: Java Performance", p. 86)

Parallel Collector은 **스루풋 콜렉터(throughput collector)**라고 알려진 방식이다.

이 방식의 목표는 다른 CPU가 대기 상태로 남아 있는 것을 최소화하는 것이다.

Serial Collector와 달리 **Young 영역에서의 콜렉션을 병렬(parallel)로 처리**한다. 많은 CPU를 사용하기 때문에 GC의 부하를 줄이고 애플리케이션의 처리량을 증가시킬 수 있다.

Old 영역의 GC는 Serial Collector와 동일하게 Mark-sweep-compact 콜렉션 알고리즘을 사용한다.

명시적으로 지정하려면 `-XX:+UseParallelGC` 를 지정하면 된다.

## Parallel Compacting Collector

- JDK 5.0 업데이트 6부터 사용 가능
- Parallel Collector와 다른 점은 Old 영역 GC에서 새로운 알고리즘을 사용한다.
    - Young 영역에 대한 GC는 동일하지만, Old 영역의 GC는 3단계를 거친다.
        - 표시 단계: 살아 있는 객체를 식별하여 표시해 놓는 단계
        - 종합 단계: 이전에 GC를 수행하여 컴팩션된 영역에 살아 있는 객체의 위치를 조사하는 단계
        - 컴팩션 단계: 컴팩션을 수행하는 단계. 수행 이후에는 컴팩션된 영역과 비어 있는 영역으로 나뉜다.
- Parallel Collector와 동일하게 여러 CPU를 사용하는 서버에 적합하다.
- GC를 사용하는 스레드 개수 지정 옵션: `-XX:ParallelGCThreads=n`
- 명시적 지정: `-XX:+UseParallelOldGC`

## Concurrent Mark-Sweep(CMS) Collector

![story17-6](/images/Book/자바성능튜닝가이드/story17-6.png)

Serial GC와 CMS GC

low-latency collector로도 알려져 있으며, 힙 메모리 영역의 크기가 클 때 적합하다.

Young 영역에 대한 GC는 Parallel Collector와 동일하다.

**Old 영역의 GC**

- **초기 표시 단계:** 매우 짧은 대기 시간으로 살아 있는 객체를 찾는 단계
- **컨커런트 표시 단계:** 서버 수행과 동시에 살아 있는 객체에 표시를 해 놓는 단계
- **재표시(remark) 단계:** 컨커런트 표시 단계에서 표시하는 동안 변경된 객체에 대해서 다시 표시하는 단계
- **컨커런트 스윕 단계:** 표시되어 있는 쓰레기를 정리하는 단계

CMS는 컴팩션 단계를 거치지 않기 때문에 왼쪽으로 메모리를 몰아 놓는 작업을 수행하지 않는다.

CMS 콜렉터 방식은 2개 이상의 프로세서를 사용하는 서버(ex, 웹 서버)에 적당하다.

`-XX:UseConcMarkSweepGC` 옵션으로 지정할 수 있다.

CMS 콜렉터는 추가적인 옵션으로 점진적 방식을 지원한다. 이 방식은 Young 영역의 GC를 더 잘게 쪼개어 서버의 대기 시작을 줄일 수 있다. CPU가 많지 않고 시스템의 대기 시간이 짧아야 할 때 사용하면 좋다. 점진적 GC를 수행하려면 `-XX:+CMSIncrementalMode` 옵션을 지정하면 된다. JVM에 따라서 `-Xincgc` 라는 옵션을 지정해도 같은 의미가 된다. 단 이 옵션은 예기치 못한 성능 저하가 발생할 수 있으니 충분한 테스트 이후 적용해야한다.

## Garbage First Collector(G1)

![story17-7](/images/Book/자바성능튜닝가이드/story17-7.png)

G1 GC의 레이아웃(이미지 출처: "The Garbage-First Garbage Collector" (TS-5419), JavaOne 2008, p. 19)

G1 Collector의 경우 지금까지의 GC와는 다른 영역으로 구성되어 있다.

- 각 바둑판의 사각형을 region이라고 한다.
- 각 영역은 최대 32MB까지 지정 가능하다.(기본 1MB)
- Young 영역과 Old 영역이 물리적으로 나뉘어 있지 않다.
- 각 영역의 크기(메모리를 의미하는 것이 아님)는 모두 동일하다.
- 이 바둑판 모양의 구역이 각 Eden, Survivor, Old 영역의 역할을 변경해 가면서 하고, Humongous 라는 영역도 포함된다.

**Young 영역의 GC**

1. 몇 개의 구역을 선정하여 Young 영역으로 지정한다.
2. Linear 하지 않은 구역에 객체가 생성되면서 데이터가 쌓인다.
3. Young 영역으로 할당된 구역에 데이터가 꽉 차면, GC를 수행한다.
4. GC를 수행하면서 살아있는 객체들만 Survivor 구역으로 이동시킨다.

이렇게 살아남은 객체들이 이동된 구역은 새로운 Survivor 영역이 된다.

다음 Young GC가 발생하면 Servivor 영역에 계속 쌓고 몇 번의 aging 작업을 통해(Survivor 영역에 있는 객체가 몇 번의 Young GC 후에도 살아 있으면), Old 영역으로 승격된다.

**Old 영역의 GC**

CMS GC의 방식과 비슷하게 6단계로 나뉜다. STW 표시된 단계는 Stop the world 수행

- **초기 표시(Initial Mark) 단계(STW):** Old 영역에 있는 객체에서 Survivor 영역의 객체를 참조하고 있는 객체들을 표시한다.
- **기본 구역 스캔(Root Region scanning) 단계:** Old 영역 참조를 위해서 Survivor 영역을 훑는다. 참고로 이 작업은 Young GC가 발생하기 전에 수행된다.
- **컨커런트 표시 단계:** 전체 힙 영역에 살아있는 객체를 찾는다. 만약 이때 Young GC가 발생하면 잠시 멈춘다.
- **재 표시(Remark) 단계(STW):** 힙에 있는 살아있는 객체들의 표시 작업을 완료한다. 이 때 snapshot-at-the-beginning(SATB)라는 알고리즘을 사용하며, 이는 CMS GC에서 사용하는 방식보다 빠르다.
- **청소(Cleaning) 단계(STW):** 살아있는 객체와 비어 있는 구역을 식별, 필요 없는 객체들을 지운다. 그리고 비어 있는 구역을 초기화 한다.
- **복사 단계(STW):** 살아있는 객체들을 비어 있는 구역으로 모은다.

G1은 CMS GC의 단점을 보완하기 위해서 만들어 졌으며 GC 성능도 매우 빠르다.

# 결론

GC는 각 영역에 할당된 크기의 메모리가 허용치를 넘을 때 수행된다.

이는 개발자가 컨트롤할 영역이 아니다.

# Reference

- [https://docs.oracle.com/javase/specs/](https://docs.oracle.com/javase/specs/)
- runtime data erea: [https://tecoble.techcourse.co.kr/post/2021-08-09-jvm-memory/](https://tecoble.techcourse.co.kr/post/2021-08-09-jvm-memory/)
- gc: [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)