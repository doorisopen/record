---
title: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가
category: Challenge
sub_category: whiteship
date: 2020-12-28 00:30:59
lastmod: 2021-01-19 00:30:59
comments: true
order: 2
---

## 목표
자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

## 학습할 것
* JVM이란 무엇인가
* 컴파일 하는 방법
* 실행하는 방법
* 바이트코드란 무엇인가
* JIT 컴파일러란 무엇이며 어떻게 동작하는지
* JVM 구성 요소
* JDK와 JRE의 차이


## JVM이란 무엇인가
**JVM(Java Virtual Machine, 자바 가상머신)** 이란 자바(Java) `바이트코드`(이후 설명)를 실핼할 수 있는 주체입니다. 쉽게 말하면 Java로 개발한 프로그램을 컴파일하여 만들어지는 **바이트코드를 실행시키기 위한 가상머신**입니다.

JVM은 `JRE(Java Runtime Environment)`에 포함되어 있으며, Java 컴파일러가 프론트엔드를 담당한다면 **자바 가상머신(JVM)**은 코드 최적화와 백엔드를 담당한다고 할 수 있습니다.

자바 가상머신이라고 해서 Java 바이트코드만 인식하는 것은 아닙니다. 바이트코드를 Java가 아닌 다른 언어들(Kotlin, Scala, Groovy)로도 생성할 수 있기 때문입니다.

> 참고 <br/>
> Java의 모토인 WORA(Write once, Run anywhere)은 JVM을 통해 가능한 것 입니다. 이 말은 JVM은 플랫폼 독립적이기 때문에 JVM이 실행 가능한 환경이라면 어디서든 Java 프로그램이 실행될 수 있다는 말입니다.
> 
> 하지만 특정 운영체제의 특수한 기능을 호출하거나 하드웨어를 제어하는 등의 일은 JVM으로 할 수 없으며, JNI 같은 Native 코드를 호출하기 위한 인터페이스를 거쳐야합니다. 일종의 샌드박스 환경(?)

## 컴파일 하는 방법
Java 코드가 컴파일되는 전체 과정을 그림으로 보면 아래와 같습니다.

![java-compilation-and-execution]({{ site.baseurl }}/images/Language/Java/java-compilation-and-execution.png)

예를 들어 다음과 같은 Java 코드가 있다고 할때,

```java
/*
 * Main.java 
 */
class Main {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

cmd 창에서 다음과 같은 명령어를 입력하여 Java 코드를 컴파일을 수행할 수 있습니다.

```sh
javac Main.java
```

Java 소스코드는 javac 컴파일러를 거쳐 .class 파일이 생성이 됩니다. 이때 .class 파일은 바이트코드를 포함하고 있습니다. 즉, javac 컴파일러가 .java 파일을 바이트코드로 변환하는 작업을 수행하는 것 입니다.


## 실행하는 방법
javac 명령어로 컴파일된 .class 파일은 다음 명령어로 실행이 가능합니다.

```sh
java Main

Hello World!
```

## 바이트코드란 무엇인가
바이트코드는 **실행 엔진(Execution Engine)**에 의해서 바이트코드를 **Native 기계 코드로 변환**하기 위한 것 입니다.

이러한 과정을 컴파일링(compiling) 이라고 하고 Java가 다른 언어에 비해 비교적 느린 이유 중 하나입니다.

## JIT 컴파일러란 무엇이며 어떻게 동작하는지
**JIT(Just-in-time) 컴파일러**는 JVM (Java Virtual Machine)의 일부입니다. 동시에 유사한 기능을 가진 바이트 코드의 일부를 해석하는 기능을 수행합니다. 그리고 컴파일 과정을 자세히 보면 아래와 같습니다.

![java-jit-converts-bytecode-into-machine-code]({{ site.baseurl }}/images/Language/Java/java-jit-converts-bytecode-into-machine-code.png)

JVM은 RAM에 위치함을 볼 수 있습니다. 그리고 실행하는 동안 javac 컴파일러로 변환된 바이트코드(Main.class)는 JRE에 들어있는 Java ClassLoader에 의해 JVM으로 적재되고 JVM은 적재된 바이트코드를 `JIT 컴파일` 방식으로 실행됩니다.

그리고 이후 **실행 엔진(Execution Engine)**에 의해서 바이트코드를 **Native 기계 코드로 변환**하게 됩니다.

## JVM 구성 요소
JVM의 구조는 아래와 같고 Class Loader, Memory Area, Excution Engine 등의 구성 요소를 포함합니다.

![jvm-architecture]({{ site.baseurl }}/images/Language/Java/java-jvm-architecture.png)

* `클래스 로더(Class Loader)`
  + 클래스 로더는 클래스 파일을 로드 하는데 사용되는 하위 시스템입니다. 클래스 로드는 로딩, 연결 및 초기화 기능을 수행합니다.
* `메소드 영역(Method Area)`
  + 메소드 영역은 메타 데이터, 상수 런타임 풀 및 메소드 코드와 같은 클래스 구조를 저장합니다.
* `힙(Heap, Heap Area)`
  + 모든 객체와 연관된 인스턴스 변수 그리고 배열 등이 힙 영역에 저장됩니다. 해당 영역의 메모리는 여러 스레드에서 공유됩니다.
* `JVM 언어 스택(JVM Language Stacks, Stack Area)`
  + 스택 영역에는 지역 변수가 저장됩니다. 스레드는 자체 JVM stack을 갖습니다.(JVM stack은 스레드가 생성되는 동시에 생성됩니다.) 메서드가 호출 될 때 마다 새 프레임(JVM Stack)이 생성되고 메서드 호출 프레서스가 완료되면 삭제됩니다.
* `PC 레지스터(PC Register)`
  + PC 레지스터는 현재 실행중인 Java 가상 머신 명령의 주소를 저장합니다. Java에서 각 스레드에는 별도의 PC 레지스터가 있습니다.
* `네이티브 메서드 스택(Native Method Stack)`
  + 네이티브 메서드 스택은 네이티브 라이브러리에 따라 네이티브 코드의 지침을 보유합니다.(Java가 아닌 언어로 작성되어있음) 
* `실행 엔진(Execution Engine)`
  + 하드웨어, 소프트웨어 또는 전체 시스템을 테스트하는데 사용되는 소프트웨어 유형입니다. 테스트 실행 엔진은 테스트 된 제품에 대한 정보를 전달하지 않습니다.
* `네이티브 메서드 인터페이스(Native Method Interface)`
  + 네이티브 메서드 인터페이스는 프로그래밍 프레임워크입니다. JVM에서 실행되는 Java 코드가 라이브러리 및 기본 애플리케이션에서 호출할 수 있습니다. 
* `네이티브 메서드 라이브러리(Native Method Libraries)`
  + 네이티브 메서드 라이브러리는 실행 엔진(Execution Engine)에 필요한 라이브러리(C,C++)의 모음입니다.

## JDK와 JRE의 차이
#### JDK(Java Development Kit)
자바 언어로 프로그램을 개발하는데 필요한 요소들이 포함된 도구 모음

#### JRE(Java Runtime Environment)
JRE는 자바 코드를 받아서 필요한 라이브러리와 결합한 다음 이 코드를 실행할 JVM을 시작하는 온디스크 시스템입니다. JRE에는 자바 프로그램 실행에 필요한 라이브러리와 소프트웨어가 포함됩니다. 예를 들어 자바 클래스 로더는 자바 런타임 환경의 일부입니다.


## References
* [live-study-week-1](https://github.com/whiteship/live-study/issues/1)
* [wiki/jvm](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0)
* [guru99/jvm](https://www.guru99.com/java-virtual-machine-jvm.html)
* [namu/jvm](https://namu.wiki/w/%EC%9E%90%EB%B0%94%20%EA%B0%80%EC%83%81%20%EB%A8%B8%EC%8B%A0?from=Java%20Virtual%20Machine)
* [itworld-jre](https://www.itworld.co.kr/t/62076/%EA%B0%80%EC%83%81%ED%99%94/110768)