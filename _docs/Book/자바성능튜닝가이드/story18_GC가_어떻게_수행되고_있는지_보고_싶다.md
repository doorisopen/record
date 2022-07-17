# story18. GC가 어떻게 수행되고 있는지 보고 싶다
생성일: 2022년 7월 9일 오후 5:51

## JDK, JRE, JVM

JDK(Java Development Kit)안에 JRE(Java Runtime Environment, 자바 실행 환경)을 포함 하고 JRE는 JVM의 실행환경을 구현한 것으로 보면 된다. 따라서 JRE안에 JVM이 포함돼 있다고 할 수 있다.

## Java 버전 별 GC(default)

- java 5, 6: Serial GC
- java 7 : Parallel GC
- java 8 : Parallel GC
- java 9 : G1 GC

GC가 어떻게 수행되는지 확인하기 위해서는 관련된 툴을 사용해야한다.

여러 방법 중 

- jstat 라는 명령을 사용하여 실시간으로 보거나
- verbosegc 옵션을 사용하여 로그로 남길 수 있다.

# Java 인스턴스 확인

`**jps`** 명령어는 운영중인 JVM의 목록을 보여준다.

JDK의 bin 디렉터리에 존재한다.(해당 명령어를 수행하기 위해 JDK 설치 디렉터리의 bin 디렉터리로 이동해야한다.)

```jsx
jps [-q] [-mlvV] [-Joption] [<hostid>]
```

- -q: 클래스나 JAR 파일명, 인수 등을 생략하고 내용을 나타낸다(단지 프로세스 id만 나타낸다)
- -m: main 메서드에 지정한 인수들을 나타낸다.
- -l: 애플리케이션의 main 클래스나 애플리케이션 JAR 파일의 전체 경로 이름을 나타낸다.
- -v: JVM에 전달된 자바 옵션 목록을 나타낸다.
- -V: JVM의 플래그 파일을 통해 전달된 인수를 나타낸다.
- -Joption: 자바 옵션을 이 옵션 뒤에 지정할 수 있다.

> 플래그 파일
.hotspotrc의 확장자를 가지거나 자바 옵션에 -XX:Flags=<file name>로 명시한 파일이다. 이 파일을 통해서 JVM의 옵션을 지정할 수 있다.
> 

 

# GC 수행 확인

`**jstat**` 명령어는 GC가 수행되는 정보를 확인하기 위한 명령어다.

```jsx
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
```

- -<option>
    - class: 클래스 로더에 대한 통계
    - compiler: 핫스팟 JIT 컴파일러에 대한 통계
    - gc: GC 힙 영역에 대한 통계
    - **gccapacity**: 각 영역의 허용치와 연관된 영역에 대한 통계
        - 각 영역의 크기를 알 수 있어 어떤 영역의 크기를 늘리고 줄여야 할지를 확인 할 수 있는 장점이 있다.
    - gccause: GC의 요약 정보와, 마지막 GC와 현재 GC에 대한 통계
    - gcnew: 각 영역에 대한 통계
    - gcnewcapacity: Young 영역과 관련된 영역에 대한 통계
    - gcold: Old와 Perm 영역에 대한 통계
    - gcoldcapacity: Old 영역의 크기에 대한 통계
    - gcpermcapacity: Perm 영역의 크기에 대한 통계
    - **gcutil**: GC에 대한 요약 정보
        - 힙 영역의 사용량을 %로 보여준다.
    - printcompilation: 핫 스팟 컴파일 메서드에 대한 통계
- -t: 수행 시간 표시
    - 자바 인스턴스가 생성된 시점부터의 시간(서버가 기동된 시점부터의 시간)
- -h:lines: 각 열의 설명을 지정된 라인 주기로 표시
- interval: 로그를 남기는 시간의 차이(밀리초 단위)
- count: 로그를 남기는 횟수

위 명령어를 사용하면 **JVM 파라미터 튜닝**을 할 때나, **GC를 수행하는 데 소요된 모든 시간**을 보고 싶을 때 유용하게 사용할 수 있다.

다만 로그를 남기는 주기에 GC가 한 번 발생할 수 도 열 번 발생할 수 있기 때문에 정확한 분석을 하고자 할 때는 `**verbosegc**` 옵션을 사용하는 것을 권장한다.

추가로 앞서 살펴본 명령어들은 로컬 시스템에서만 사용할 수 있는 명령어들이고 원격지의 시스템을 모니터링 하기 위한 명령어는 `**jstatd**`라는 명령어를 사용하면 된다.

# varbosegc로 gc로그 남기기

**사용법**

```jsx
java -verbosegc <기타 다른 옵션> 자바 애플리케이션 이름
```

**옵션**

- **PrintGCTimeStamps(옵션: -XX:+PrintGCTimeStamps)**
    - GC가 언제 발생됐는지 수행한 시간이 포함된다.
- **PrintHeapAtGC(옵션: -XX:+PrintHeapAtGC)**
    - GC가 수행되고 각 영역(Eden, Survival, Old, 전체 영역)에서 얼마나 많은 메모리 영역을 사용하고 있는지 전, 후를 출력한다.
    - PrintGCTimeStamps 옵션보다 더 많은 정보를 볼 수 있다.
    - 너무 많은 내용을 보여줘서 분석의 어려움이 있을 수 있다.
- **PrintGCDetails(옵션: -XX:PrintGCDetail)**
    - GC 발생 결과를 간결하게 보여준다.
    

## 다양한 분석 툴

- GC Analyzer
    - Sun에서 제공하는 GC 분석 툴
    - Perl 스크립트 기반으로 분석 결과를 정리
- IBM GC 분석기(IBM Pattern Modeling and Analysis Tool for Java Garbage Collector, IBM PMAT)
    - IBM에서 제공하는 GC 분석 툴
    - Sun, HP 서버에서 작성된 GC 로그를 분석할 수 있다.
- HPjtune
    - HP에서 제공하는 GC 분석 툴
    - 오직 HP JDK를 사용하여 작성된 GC 로그만 분석할 수 있다.
    - 아파치 그룹의 JMeter와 전혀 무관한 툴이다.
    

# 정리

메모리 릭은 Full GC 이후 메모리의 변화를 확인해보자 

Full GC 이후에도 Old 영역의 메모리 점유율이 줄어들지 않고 높은 상태라면 그 땐 메모리 릭을 의심해보자

그러나 요즘은 프레임워크를 사용하기 때문에 개발자가 실수하는 일이 줄어 들었고 메모리 릭이 발생하는 경우는 1% 에 불과하다.

# Reference

- [https://bongbong-89.tistory.com/10](https://bongbong-89.tistory.com/10)