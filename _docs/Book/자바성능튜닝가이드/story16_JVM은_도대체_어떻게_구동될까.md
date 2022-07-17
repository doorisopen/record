# story16. JVM은 도대체 어떻게 구동될까?
생성일: 2022년 6월 28일 오후 11:38

---

웹 애플리케이션을 배포할 때, 배포 직후 시스템 사용자들은 엄청나게 느린 응답 시간을 경험할 수 있다. 즉 배포 직후 Warming up이 필요한데, 왜 이런 작업이 필요한지 알아보자

알아두면 좋을 주요 내용

- HotSpot VM의 구조
- JIT 옵티마이저
- JVM의 구동 절차
- JVM의 종료 절차
- 클래스 로딩의 절차
- 예외 처리의 절차

[참고] 주요 용어 정리: [https://openjdk.org/groups/hotspot/docs/HotSpotGlossary.html](https://openjdk.org/groups/hotspot/docs/HotSpotGlossary.html)

## [참고] JVM 구조

![story16-1](/images/Book/자바성능튜닝가이드/story16-1.png)

| Class Loader | - Runtime Data Area에 Java Class를 로드 
로드(Loading) → 링크(Linking) → 초기화(Initialization) |
| --- | --- |
| Runtime Data Area | OS로부터 할당받는 메모리 영역(JVM 메모리 구조)
- Method Area: 클래스의 메타 정보, 모든 Thread가 공유 
- Heap Area: 모든 객체의 인스턴스와 배열, 모든 Thread가 공유 
- Stack Area: 메서드가 호출될 때 수행 정보, Thread마다 존재 
- PC Register: 현재 수행 중인 JVM 명령의 주소, Thread마다 존재 
- Native Method Stack: Java 외의 언어로 작성된 네이티브 코드, Thread마다 존재 |
| Execution Engine | - byte code를 기계어로 변환하여 명령어 단위로 실행 
- 명령어를 하나씩 수행하는 인터프리터 방식과 프로그램을 실제 실행하는 시점에 기계어로 번역하는 JIT 컴파일러 방식이 있는데 두 가지 방식을 혼용해서 사용 |

## [참고] JVM 종류

| 종류 | 개발사 |
| --- | --- |
| HotSpot | Sun Microsystems |
| JRockit | BEA System |
| Eclipse OpenJ9 | IMB J9 기반 |
| Kafee | 클린 룸 |
| IMB J9 | IBM |

윈도우, 리눅스 등의 환경에서는 대부분 HotSpot이 사용되고 IBM AIX에서는 IBM J9가 사용된다.

# HotSpot VM은 어떻게 구성되어 있을까?

HotSpot이라는 단어는 자바에서 Java HotSpot Performance Engine라는 명칭으로 사용한다.

자바를 만든 Sun에서는 **자바의 성능을 개선하기 위해서 `Just In Time(JIT) 컴파일러`를 만들었고, 이름을 `HotSpot`으로 지었다.**

여기서 **JIT 컴파일러**는 프로그램의 성능에 영향을 주는 지점에 대해서 지속적으로 분석하고 분석된 지점은 부하를 최소화하고, 높은 성능을 내기 위한 최적화의 대상이 된다.

> **JIT를 사용한다는 것은 ‘언제나 자바 메서드가 호출되면 바이트 코드를 컴파일하고 실행 가능한 네이티브 코드로 변환한다’는 의미**다. 하지만 매번 JIT로 컴파일을 하면 성능 저하가 심하므로, 최적화 단계를 거치게 된다.
> 

HotSpot은 자바 1.3 버전부터 기본 VM으로 사용되어 왔기 때문에, 지금 운영되고 있는 대부분의 시스템들은 모두 HotSpot 기반의 VM이라고 보면 된다.

**HotSpot VM의 주요 컴포넌트**

- VM(Virtual Machine) 런타임
- JIT(Just In Time) 컴파일러
- 메모리 관리자

![story16-2](/images/Book/자바성능튜닝가이드/story16-2.png)

- HotSpot VM은 **‘HotSpot VM 런타임’**에 **‘GC 방식’**과 **‘JIT 컴파일러’**를 끼워 맞춰 사용할 수 있다.
- VM 런타임은 **JIT 컴파일러용 API와 가비지 컬렉터용 API를 제공**한다.
- JVM을 시작하는 런처와 스레드 관리, JNI 등도 VM 런타임에서 제공한다.

**용어 설명**

- **JNI(Java Native Interface)**: Java 코드가 네이티브 C 코드를 호출하는 방법과 네이티브 C 코드가 Java VM을 호출하는 방법에 대한 사양 및 API

## JIT Optimizer

JIT Compiler는 Client 버전과 Server 버전으로 나뉜다.

**컴파일**이라는 작업은 상위 레벨의 언어로 만들어진 기계에 의존적인 코드로 변환하는 것을 말한다.

**자바의 경우**, javac라는 컴파일러를 사용한다. 이 컴파일러는 소스코드를 바이트 코드로 된 class라는 파일로 변환해준다. 그렇기 때문에 JVM은 항상 바이트 코드로 시작하며, 동적으로 기계에 의존적인 코드로 변환한다.

**JIT**는 애플리케이션에서 각각의 메서드를 컴파일할 만큼 시간적 여유가 많지 않다. 그러므로, **모든 코드는 초기에 인터프리터에 의해서 시작되고, 해당 코드가 충분히 많이 사용될 경우에 컴파일할 대상이 된다.** HotSpot VM에서 이 작업은 각 메서드에 있는 카운터를 통해서 통제되며, 메서드에는 두 개의 카운터가 존재한다.

- **수행 카운터(invacation counter)**: 메서드를 시작할 때마다 증가
- **백에지 카운터(backedge counter)**: 높은 바이트 코드 인덱스에서 낮은 인덱스로 컨트롤 흐름이 변경될 때마다 증가

**백에지 카운터**는 메서드가 루프가 존재하는지를 확인할 때 사용되며, **수행 카운터** 보다 컴파일 우선순위가 높다.

이 카운터들이 인터프리터에 의해서 증가될 때마다, 그 값들이 한계치에 도달했는지를 확인하고, 도달했을 경우 인터프리터는 컴파일을 요청한다.

> **수행 카운터**에서 사용하는 한계치는 **CompileThreshold**며, **백에지 카운터**에서 사용하는 한계치는 다음 공식으로 계산한다.
`CompileThreshold * OnStackReplacePercentage / 100`

이 두 개의 값들은 JVM이 시작할 때 지정 가능하며 다음과 같이 시작 옵션에 지정할 수 있다.
▪️ XX:CompileThreshold=35000
▪️ XX:OnStackReplacePercentage=80

즉, 이렇게 지정하면 메서드가 3만 5천번 호출됐을 때 JIT에서 컴파일을 하며, 백에지 카운터가 35,000 * 80 / 100 = 28,000이 되었을 때 컴파일된다.
> 

**컴파일이 요청되면**

- 컴파일 대상 목록의 큐에 쌓이고, 하나 이상의 컴파일러 스레드가 이 큐를 모니터링 한다.
- 만약 컴파일러 스레드가 바쁘지 않을 때는 큐에서 대상을 빼내서 컴파일을 시작한다.
- 이때 **인터프리터**는 컴파일이 종료되기를 기다리지 않는 대신, 수행 카운터를 리셋하고 인터프리터에서 메서드 수행을 계속한다.

**컴파일이 종료되면**

- 컴파일된 코드와 메서드가 연결되어 그 이후 부터는 메서드가 호출되면 컴파일된 코드를 사용하게 된다.
- 만약 인터프리터에서 컴파일이 종료될 때까지 기다리도록 하려면, JVM 시작시 -Xbatch나 -XX:-BackgroundCompilation 옵션을 지정하여 기다리도록 할 수 있다.

HotSpot VM은 **OSR(On Stack Replacement)라는 특별한 컴파일도 수행**한다.

- OSR은 인터프리터에서 수행한 코드 중 오랫동안 루프가 지속되는 경우에 사용된다.
- 만약 **코드의 컴파일이 완료된 상태**에서 최적화되지 않은 코드가 수행되고 있는 것을 발견한 경우에 인터프리터에 계속 머무르지 않고 컴파일된 코드로 변경한다.
- 인터프리터에서 시작된 오랫동안 지속되는 루프가 다시는 불리지 않을 경우엔 도움이 되지 않지만, 루프가 끝나지 않고 지속적으로 수행되고 있을 경우에는 큰 도움이 된다.

> Java 5 HotSpot VM이 발표되면서 새로운 기능이 추가되었다.
▪️ JVM이 시작될 때 **플랫폼**과 **시스템 설정**을 평가하여 자동으로 garbage collector를 선정하고, 자바 힙 크기와 JIT 컴파일러를 선택하는 것이다. 
▪️ 이 기능을 통해서 애플리케이션의 활동과 객체 할당 비율에 따라서 **garbage collector가 동적으로 자바 힙 크기를 조절**하며, **New의 Eden과 Survivor, Old 영역의 비율을 자동적으로 조절**할 수 있다.
▪️ 이 기능은 -XX:+UseParallelGC와 -XX:+UseParallelOldGC에서만 적용되며, 이 기능을 제거하려면 -XX:-UseAdaptiveSizePolicy라는 옵션을 적용하여 끌 수가 있다.
> 

# JIT 컴파일 및 최적화 절차

JRockit JVM, IBM JVM의 JIT 최적화 내용 설명

책 p302~308 참고

# JVM 수행 절차

## JVM 시작

java 명령어로 HelloWorld 클래스를 실행과정

1. java 명령어 줄에 있는 옵션 파싱
    1. 일부 명령은 자바 실행 프로그램에서 적절한 JIT 컴파일러를 선택하는 등의 작업을 하기 위해서 사용하고, 다른 명령들은 HotSpot VM에 전달된다.
2. 자바 힙 크기 할당 및 JIT 컴파일러 타입 지정:(이 옵션들이 명령줄에 지정되지 않았을 경우)
    1. 메모리 크기나 JIT 컴파일러 종류가 명시적으로 지정되지 않은 경우에 자바 실행 프로그램이 시스템의 상황에 맞게 선정한다. 이 관정은 좀 복잡한 단계(HotSpot VM Adaptive Tuning)를 거치니 일단 넘어간다.
3. CLASSPATH와 LD_LIBRARY_PATH같은 환경 변수를 지정한다.
4. 자바의 Main 클래스가 지정되지 않았으면, Jar 파일의 manifest 파일에서 Main 클래스를 확인한다.
5. JNI의 표준 API인 JNI_CreateJavaVM을 사용하여 새로 생성한 non-primordial이라는 스레드에서 HotSpot VM을 생성한다.
    - JNI_CreateJavaVM 단계
        1. JNI_CreateJavaVM는 동시에 두개의 스레드에서 호출할 수 없고, 오직 하나의 HotSpot VM 인스턴스가 프로세스 내에서 생성될 수 있도록 보장된다. HotSpot VM이 정적인 데이터 구조를 생성하기 때문에 다시 초기화는 불가능하기 때문에, 오직 하나의 HotSpot VM이 프로세스에서 생성될 수 있다.
        2. JNI 버전이 호환성이 있는지 점검하고, GC 로깅을 위한 준비도 완료된다.
        3. OS 모듈들이 초기화된다. 예를 들면 랜덤 번호 생성기, PID 할당 등이 여기에 속한다.
        4. 커맨드 라인 변수와 속성들이 JNI_CreateJavaVM 변수에 전달되고, 나중에 사용하기 위해서 파싱한 후 보관한다.
        5. 표준 자바 시스템 속성(properties)이 초기화된다.
        6. 동기화, 메모리, safepoint 페이지와 같은 모듈들이 초기화된다.
        7. libzip, libhpi, libjava, libthread와 같은 라이브러리들이 로드된다.
        8. 시그널 처리기가 초기화 및 설정된다.
        9. 스레드 라이브러리가 초기화된다.
        10. 출력(output) 스트림 로거가 초기화된다.
        11. JVM을 모니터링하기 위한 에이전트 라이브러리가 설정되어 있으면 초기화 및 시작된다.
        12. 스레드 처리를 위해서 필요한 스레드 상태와 스레드 로컬 저장소가 초기화된다.
        13. HotSpot VM의 ‘글로벌 데이터’들이 초기화된다. 글로벌 데이터에는 이벤트 로그(event log), OS 동기화, 성능 통계 메모리(perfMemory), 메모리 할당자(chunkPool)들이 있다.
        14. HotSpot VM에서 스레드를 생성할 수 있는 상태가 된다. main 스레드가 생성되고, 현재 OS 스레드에 붙는다. 그러나 아직 스레드 목록에 추가되지는 않는다.
        15. 자바 레벨의 동기화가 초기화 및 활성화된다.
        16. 부트 클래스로더, 코드 캐시, 인터프리터, JIT 컴파일러, JNI, 시스템 dictionary, ‘글로벌 데이터’ 구조의 집한인 universe 등이 초기화된다.
        17. 스레드 목록에 자바 main 스레드가 추가되고, universe의 상태를 점검한다. HotSpot VM의 중요한 기능을 하는 HotSpot VMThread가 생성된다. 이 시점에 HotSpot VM의 현재 상태를 JVMTI에 전달한다.
        18. java.lang 패키지에 있는 String, System, Thread, ThreadGroup, Class 클래스와 java.lang의 하위 패키지에 있는  Method, Finalizer 클래스 등이 로딩되고 초기화된다.
        19. HotSpot VM의 시그널 핸들러 스레드가 시작되고, JIT 컴파일러가 초기화되며, HotSpot의 컴파일 브로커 스레드가 시작된다. 그리고 HotSpot VM과 관련된 각종 스레드들이 시작한다. 이때부터 HotSpot VM의 전체 기능이 동작한다.
        20. JNIEnv가 시작되며, HotSpot VM을 시작한 호출자에게 새로운 JNI 요청을 처리할 상황이 되었다고 전달해 준다.
6. HotSpot VM이 생성되고 초기화되면, Main 클래스가 로딩된 런처에서는 main()메서드의 속성 정보를 읽는다.
7. CallStaticVoidMethod는 네이티브 인터페이스를 불러 HotSpot VM에 있는 main()메서드가 수행된다. 이때 자바 실행 시 Main 클래스 뒤에 있는 값들이 전달된다.

## JVM 종료

정상적으로 JVM을 종료 시킬 때는 다음의 절차를 거치지만, OS의 kill -9와 같은 명령으로 JVM을 종료 시키면 이 절차를 따르지 않는다

만약 JVM이 시작할 때 오류가 있어 시작을 중지할 때나, JVM에 심각한 에러가 있어서 중지할 필요가 있을 때는 DestroyJavaVM이라는 메서드를 HotSpot 런처에서 호출한다. HotSpot VM의 종료는 다음의 DestoryJavaVM 메서드의 종료 절차를 따른다.

1. HotSpot VM이 작동중인 상황에서는 단 하나의 데몬이 아닌 스레드(nondaemon thread)가 수행될 때까지 대기한다.
2. java.lang 패키지에 있는 Shutdown 클래스의 shutdown() 메서드가 수행된다. 이 메서드가 수행되면 자바 레벨의 shutdown hook 이 수행되고, finalization-on-exit이라는 값이 true일 경우에 자바 객체 finalizer를 수행한다.
3. HotSpot VM 레벨의 shutdown hook을 수행함으로써 HotSpot VM의 종료를 준비한다. 이 작업은 JVM_OnExit()메서드를 통해서 지정된다. 그리고 HotSpot VM의 profiler, stat smapler, watcher, garbage collector 스레드를 종료시킨다. 이 작업들이 종료되면 JVMTI를 비활성화하며, Signal 스레드를 종료시킨다.
4. HotSpot의 JavaThread::exit() 메서드를 호출하여 JNI 처리 블록을 해제한다. 그리고, guard pages, 스레드 목룍에 있는 스레드들을 삭제한다. 이 순간 부터는 HotSpot VM에서는 자바 코드를 실행하지 못한다.
5. HotSpot VM 스레드를 종료한다. 이 작업을 수행하면 HotSpot VM에 남아있는 HotSpot VM 스레드를을 safepoint로 옮기고, JIT 컴파일러 스레드들을 중지시킨다.
6. JNI, HotSpot VM, JVMTI barrier에 있는 추적(tracing) 기능을 종료시킨다.
7. 네이티브 스레드에서 수행하고 있는 스레드들을 위해서 HotSpot의 “vm exited” 값을 설정한다.
8. 현재 스레드를 삭제한다
9. 입출력 스트림을 삭제하고, PerfMemory 리소스 연결을 해제한다.
10. JVM 종료를 호출한 호출자로 복귀한다.

## 클래스 로딩

자바 클래스가 메모리에 로딩되는 절차

1. 주어진 클래스의 이름으로 클래스 패스에 있는 바이너리로 된 자바 클래스를 찾는다.
2. 자바 클래스를 정의한다.
3. 해당 클래스를 나타내는 java.lang 패키지의 Class 클래스의 객체를 생성한다.
4. 링크 작업이 수행된다. 이 단계에서 static 필드를 생성 및 초기화하고, 메서드 테이블을 할당한다.
5. 클래스의 초기화가 진행되며, 클래스의 static 블록과 static 필드가 가장 먼저 초기화 된다. 당연한 이야기지만, 해당 클래스가 초기화 되기 전에 부모 클래스의 초기화가 먼저 이루어진다.

> loading → linking → linitializing
> 

### 클래스 로딩 시 발생하는 에러

일반적으로 이 에러들은 자주 발생하지 않는다.

- NoClassDefFoundError: 만약 클래스 파일을 찾지 못할 경우
- ClassFormatError: 클래스 파일의 포멧이 잘못된 경우
- UnsupportedClassVersionError: 상위 버전의 JDK 에서 컴파일한 클래스를 하위 버전의 JDK에서 실행하려고 하는 경우
- ClassCircularityError: 부모 클래스르 로딩하는 데 문제가 있는 경우(자바는 클래스를 로딩하기 전에 부모 클래스들을 미리 로딩해야 한다.)
- IncompatibleClassChangeError: 부모가 클래스인데 implements를 하는 경우나 부모가 인터페이스인데 extends하는 경우
- VerifyError: 클래스 파일의 semantic, 상수 풀, 타입 등의 문제가 있을 경우

---

- 클래스 로더가 클래스를 찾고 로딩할 때 다른 클래스 로더에 클래스를 로딩해 달라고 하는 경우가 있다. 이를 ‘class loader delegation’이라고 부른다.
- 클래스 로더는 계층적으로 구성되어 있다.
    - 기본 클래스 로더는 ‘시스템 클래스 로더’라고 불리며 main 메서드가 있는 클래스와 클래스 패스에 있는 클래스들이 이 클래스 로더에 속한다.
    - 그 하위에 있는 애플리케이션 클래스 로더는 자바 SE의 기본 라이브러리에 있는 것이 될 수도 있고, 개발자가 임의로 만든 것일 수도 있다.

### 부트스트랩(Bootstrap) 클래스 로더

HotSpot VM은 부트스트랩 클래스 로더를 구현한다. 부트스트랩 클래스 로더는 HotSpot VM의 BOOTCLASSPATH에서 클래스들을 로드한다. 예를 들면, Java SE(Standard Edition)클래스 라이브러리들을 포함하는 rt.jar가 여기에 속한다.  

> 보다 빠른 JVM의 시작을 위해서 **클라이언트 HotSpot VM**은 ‘클래스 데이터 공유’라고 불리는 기능을 통해서 클래스를 미리 로딩할 수도 있으며, 이 기능은 기본적으로 켜져 있다.
`-Xshare:on`, `-Xshare:off`
그러나, 서버 HotSpot VM은 ‘클래스 데이터 공유’ 기능을 제공하지 않고, 또 클라이언트 HotSpot VM도 시리얼 GC를 사용하지 않을 경우에는 이 기능을 제공하지 않는다.
> 

### HotSpot의 클래스 메타데이터(Metadata)

HotSpot VM 내에서 클래스를 로딩하면 클래스에 대한 instanceKlass와 arrayKlass라는 내부적인 형식을 VM의 Perm 영역에 생성한다.

instanceKlass는 클래스의 정보를 포함하는 java.lang.Class 클래스의 인스턴스를 말한다.

HotSpot VM은 내부 데이터 구조인 klassOop라는 것을 사용하여 내부적으로 instanceKlass에 접근한다. 여기서 Oop라는 것은 ordinary object pointer의 약자다. 즉, klassOop는 클래스를 나타내는 ordinary object pointer를 의미한다.

### 내부 클래스 로딩 데이터의 관리

HotSpot VM은 클래스 로딩을 추적하기 위해서 다음의 3개의 해시 테이블을 관리한다.

- SystemDictionary
    - 로드된 클래스를 포함하며, 클래스 이름 및 클래스 로더를 키를 갖고 그 값으로 klassOop를 갖고 있다. SystemDictionary는 클래스 이름과 초기화한 로더의 정보, 클래스 이름과 정의한 로더의 정보도 포함한다. 이 정보들은 safepoint에서만 제거된다.
- PlaceholderTable
    - 현재 로딩된 클래스들에 대한 정보를 관리한다. 이 테이블은 ClassCircularity Error를 체크할 때 사용하며, 다중 스레드에서 클래스를 로딩하는 클래스 로더에서도 사용된다.
- LoaderConstraintTable
    - 타입 체크시의 체약 사항을 추정하는 용도로 사용된다.

## 예외 발생 시 JVM

JVM은 자바 언어의 제약을 어겼을 때 예외(exception)라는 시그널(signal)로 처리한다. HotSpot VM 인터프리터, JIT 컴파일러 및 다른 HotSpot VM 컴포넌트는 예외 처리와 모두 관련되어 있다. 일반적인 예외 처리 경우는 아래 두 가지 경우다.

- 예외를 발생한 메서드에서 잡을 경우
- 호출한 메서드에 의해서 잡힐 경우

후자의 경우에는 보다 복잡하며, 스택을 뒤져서 적당한 핸들러를 찾는 작업을 필요로한다.

- 던져진 바이트 코드에 의해서 초기화 될 수 있으며,
- VM 내부 호출의 결과로 넘어올 수도 있고,
- JNI 호출로 부터 넘어올 수도 있고,
- 자바 호출로부터 넘어올 수도 있다.

VM이 예외가 던져졌다는 것을 알아차렸을 때, 해당 예외를 처리하는 가장 가까운 핸들러를 찾기 위해서 HotSpot VM 런타임 시스템이 수행된다. 이 때 핸들러를 찾기 위해서는 다음의 3개의 정보가 사용된다.

- 현재 메서드
- 현재 바이트 코드
- 예외 객체

만약 현재 메서드에서 핸들러를 찾기 못했을 때는 현재 수행되는 스택 프레임을 통해서 이전 프레임을 찾는 작업을 수행한다. 적당한 핸들러를 찾으면 HotSpot VM 수행 상태가 변경되며, HotSpot VM은 핸들러로 이동하고 자바 코드 수행은 계속된다.

# Reference

- JVM: [https://namu.wiki/w/자바 가상 머신](https://namu.wiki/w/%EC%9E%90%EB%B0%94%20%EA%B0%80%EC%83%81%20%EB%A8%B8%EC%8B%A0)
- [https://i3utterfly.tistory.com/entry/JVMJava-Virtual-Machine-란](https://i3utterfly.tistory.com/entry/JVMJava-Virtual-Machine-%EB%9E%80)