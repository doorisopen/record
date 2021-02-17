---
title: 자바 데이터 타입, 변수 그리고 배열
category: Challenge
sub_category: whiteship
date: 2021-02-10 00:30:59
lastmod: 2021-02-10 00:30:59
comments: true
order: 3
---


## 목표
자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익힙니다.

## 학습할 것
* 프리미티브 타입 종류와 값의 범위 그리고 기본 값
* 프리미티브 타입과 레퍼런스 타입
* 리터럴
* 변수 선언 및 초기화하는 방법
* 변수의 스코프와 라이프타임
* 타입 변환, 캐스팅 그리고 타입 프로모션
* 1차 및 2차 배열 선언하기
* 타입 추론, var


## 프리미티브 타입 종류와 값의 범위 그리고 기본 값
|타입 종류|데이터 타입|메모리 크기|기본 값|값의 표현 범위|
|:---:|:---:|:---:|:---:|:---:|
|논리형|boolean|1 byte|false|$\small{true,false}$|
|정수형|byte|1 byte|0|$\small{-128 \sim 127}$|
||short|2 byte|0|$\small{-32,768 \sim 32,767}$|
||int|4 byte|0|$\small{-2,147,483,648 \sim 2,147,483,647}$|
||long|8 byte|0|$\small{-9,223,372,036,854,775,808 \sim 9,223,372,036,854,775,807}$|
||char|2 byte(유니코드)|'\u0000'|$\small{0\sim65,535}$|
|실수형|float|4 byte|0.0F|$\small{3.4\times10^{-38}}\sim{3.4\times10^{38}}$|
||double|8 byte|0.0|$\small{1.7\times10^{-308}}\sim{1.7\times10^{308}}$|

## 프리미티브 타입과 레퍼런스 타입
자바의 데이터 타입은 크게 **프리미티브(Primitive) 타입과 레퍼런스(Reference) 타입**으로 구분합니다.

![java-data-type]({{ site.baseurl }}/images/Language/Java/java-data-type.png)

## 리터럴
Java에서 소스 코드 내의 데이터 값 그대로 쓴 상수를 **리터럴(literal)** 이라고 부릅니다.

```java
class Main {
    public static void main(String[] args) {
        int num = 4; // '4': int 타입 리터럴
        double sum = num + 0.5 // '0.5': double 타입 리터럴
        System.out.println("sum=" + sum); // 'sum=': String 타입 리터럴
        System.out.println('끝'); // '끝': char 타입 리터럴
    }
}
```

자바 컴파일러는 기본적으로 아래와 같은 경우에 해당 리터럴을 부여합니다.

* 소수점이 없는 수치(ex: 1,2,3,..) 리터럴은 **int 타입**을
* 소수점이 있는 수치(ex: 0.5,0.25,..) 리터럴은 **double 타입**을
* 큰따옴표("")로 묶여진 텍스트에는 **String 타입**을
* 작은따옴표('')로 묶여진 문자에는 **char 타입**을 부여합니다.

#### 정수 리터럴
소수점이 없는 10진수 리터럴은 int타입으로 간주합니다. 그런데 주의할 점이 **0으로 시작되는 정수는 10진수 처럼 보여도 8진수로 해석됩니다.**

그리고, **0x 혹은 0X로 시작하는 정수 리터럴은 16진수로 취급합니다.**

아래 코드를 통해 알아보겠습니다.

```java
class Main {
    public static void main(String[] args) {
        System.out.println(12); // 120 (10진수)
        System.out.println(024); // 20 (8진수로 취급함)
        System.out.println(0x30A1); // 12449 (16진수로 취급함)
        System.out.println(0x0030a1); // 12449 (16진수로 취급함)
    }
}
```

추가로 int 타입의 범위$\small{(-2,147,483,648 \sim 2,147,483,647)}$를 벗어나는 경우에 long 타입으로 표기할 수 있는데 이때 **정수 리터럴의 마지막에 대문자 L이나 소문자 l을 쓰면 long 타입 리터럴로 간주합니다.**(대문자 L 사용을 선호합니다.)

```java
long num = 120L;
long num = 120l;
long num = 0x30A1L;
```

> 참고로 byte, short 타입의 리터럴 표기법은 따로 없고 int 타입에서 byte, short 타입으로 변환하는 방법으로 사용합니다.

#### 부동소수점 리터럴
소수점이 있는 정수 리터럴은 double 타입으로 간주합니다. 또한, double 타입 리터럴의 경우 정수부나 소수부 중 한쪽이 0일 경우에 생략할 수 있습니다.

```java
12. // == 12.0
.025 // == 0.025
```

다음은 double 타입 리터럴과 float 타입 리터럴 표기 방법에 대한 코드입니다.

```java
double num1 = 12e100; // 12*10^100 
double num2 = 0.25E-20; // 0.25*10^-20

float num3 = 12.025F;
float num4 = 12.f;
float num5 = 12e10F;
float num6 = 12F;
```

위와 같이 float 타입의 경우 리터럴의 마지막에 F 혹은 f를 쓰면 float 타입으로 간주합니다.

참고로 부동소수점 리터럴도 정수 리터럴처럼 16진법의 지수, 가수로 표현할 수 있습니다. 위의 **지수를 표현하는 E 혹은 e 대신 P 혹은 p 를 써야합니다.**

```java
double num1 = 0xA1.27p5;
float num2 = 0xA1.27p5f;
```

#### 문자 리터럴
문자 리터럴의 경우 String 혹은 char 타입의 리터럴이 있습니다. String 타입의 경우 큰따옴표("")로 감싸진 경우에 String 타입의 리터럴로 간주합니다.

char 타입의 경우 작은따옴표('')로 감싸진 경우 char 타입의 리터럴로 간주합니다. 추가로 어떤 문자는 소스 코드에 쓸 수 없기 떄문에 **[escape sequence](https://docs.microsoft.com/ko-kr/cpp/c-language/escape-sequences?view=msvc-160)**로 표기합니다.

그리고 char 타입 리터럴 중 Unicode 문자는 \u 혹은 \U 쓰고 유니코드 코드 값 4자리의 16진수로 써서 표현할 수 있습니다.(ex: \uC790,...)

#### 불리언 리터럴
true 혹은 false 값은 boolean 타입의 리터럴로 취급됩니다.

## 변수 선언 및 초기화하는 방법

```java
class Main {
    public static void main(String[] args) {
        int num1 = 1;
        double num2 = 1.5;
        float num3 = 1.5f;
        String str1 = "white ship";
        char c1 = '~';
        boolean b1 = true;
    }
}
```

## 변수의 스코프와 라이프타임
**변수의 스코프**는 변수에 접근할 수 있는 범위, 영역을 의미합니다. 일반적으로 변수가 선언된 블록(중괄호: {})내에서만 액세스가 가능합니다.

**변수의 라이프타임**은 변수가 메모리에 살아있는 기간을 의미합니다.

변수는 아래와 같은 영역에 선언될 수 있습니다.

* 인스턴스(instance) 변수(맴버)
* 클래스(static) 변수
* 지역(local) 변수

```java
public class Member { // 클래스
    /*필드*/
    String name; // 맴버 변수, 인스턴스 변수, 클래스 변수
    int salary;
    static int age; // 클래스 변수(static 키워드가 붙으면 클래스 변수)

    /*메서드*/
    public void updateName(String name) { // name은 매개 변수
        this.name = name;
    }

    public void updateSalary() {
        int increaseRate = 100; // 지역변수
        this.salary += increaseRate;
    }
}

public class Main {
    public static void main(String[] args) {
        Member member = new Member();//클래스 객체(=인스턴스)
        member.name = "김철수"; // 레퍼런스 변수.멤버 변수
        member.salary = 100; // 레퍼런스 변수.멤버 변수
        member.updateSalary(); // 레퍼런스 변수.메서드
    }
}
```

#### 인스턴스 변수(Instance Variables)

```java
public class Member { // 클래스
    /*필드*/
    String name; // 맴버 변수, 인스턴스 변수, 클래스 변수
    int salary;
```

클래스 내부와 모든 메소드 및 블록 외부에서 선언된 변수들을 말합니다.

* **스코프**
  + 정적(static) 메소드를 제외한 클래스 전체 
* **라이프타임**
  + 객체가 메모리에 남아있을 때까지 


#### 클래스 변수(Class Variables)

```java
public class Member { // 클래스
    /*필드*/
    ...(생략)
    static int age;
```

클래스 내부에서 static 키워드가 붙은 변수

* **스코프**
  + 클래스 전체
* **라이프타임**
  + 클래스가 메모리에 로드 되는 동안, 프로그램이 종료될때까지

#### 지역 변수(Local Variables)

```java
public class Member { // 클래스
    /*필드*/
    ...(생략)
    /*메서드*/
    public void updateName(String name) { // name은 매개 변수
        this.name = name;
    } // name의 라이프타임은 해당 블록을 벗어나기 전까지입니다.
    
    public void updateSalary() {
        int increaseRate = 100; // 지역변수
        this.salary += increaseRate;
    } // increaseRate의 라이프타임은 해당 블록을 벗어나기 전까지입니다.
}
```

인스턴스 및 클래스 변수가 아닌 모든 변수

* **스코프**
  + 선언된 블록 이내
* **라이프타임**
  + 블록을 벗어나기 전까지

## 타입 변환, 캐스팅 그리고 타입 프로모션
타입 변환에는 두 종류가 있습니다.

* 자동 타입 변환(타입 프로모션, 묵시적)
* 강제 타입 변환(캐스팅, 명시적)

아래는 정수형 타입의 크기별 순서입니다.(타입명(byte))

**byte(1) < short(2) < int(4) < long(8) < float(4) < double(8)**

#### 자동 타입 변환(타입 프로모션, 묵시적)
**자동 타입 변환(프로모션)** 이란 **크기가 더 작은 자료형을 더 큰 자료형에 대입할 때, 자동으로 작은 자료형이 큰 자료형으로 변환**되는 것을 말합니다.

```java
int num1 = 10;
float num2 = num1;
System.out.println(num1);// 10
System.out.println(num2);// 10.0
```

> 음수 저장이 가능한 byte, int 등의 타입은 char 타입으로 자동 타입 변환이 불가능 합니다.(컴파일 에러 발생)

#### 강제 타입 변환(캐스팅, 명시적)
**강제 타입 변환(캐스팅)** 이란 **크기가 더 큰 자료형을 더 작은 자료형에 대입할 때, 자료형을 명시해서 강제로 집어넣는 것**을 말합니다.

```java
float num1 = 10;
int num2 = num1; // Error!!!
int num3 = (int) num1; // OK

System.out.println(num1);// 10.0
System.out.println(num3);// 10
```

## 1차 및 2차 배열 선언하기
1차원 배열은 아래와 같이 선언할 수 있습니다.

```java
// 1.타입[] 변수명
int[] nums;

// 2.타입 변수명[]
String strs[];
```

그러나 위의 선언된 배열에 데이터를 저장하려고 하면 **NullPointerException** 을 보게 됩니다. 이유는 **배열을 선언한 것이지 배열을 생성한 것이 아니기 때문**입니다. 아래와 같이 배열을 생성해줍니다.

```java
// 변수명 = new 타입[배열 크기]
nums = new int[10]; // 4byte 크기의 배열 10개 생성
strs = new String[10];
```

2차원 배열은 아래와 같이 1차원 배열과 동일한 방법으로 선언 및 생성할 수 있습니다.

```java
// 1.타입[][] 변수명 = new 타입[][]
int[][] nums = new int[10][10];

// 2.타입 변수명[][] = new 타입[][]
String strs[][] = new String[10][10];
```

추가로 **배열의 인덱스는 0부터 시작하고 아래와 같이 선언과 초기화를 한번에 가능**합니다.

```java
int[] nums = {1,2,3,4,5}; // 4byte 크기의 배열 5개 생성
System.out.println(nums[0]); // 1
System.out.println(nums[1]); // 2
System.out.println(nums[4]); // 5
```

## 타입 추론, var
**타입 추론**이란 타입이 정해지지 않은 변수에 대해서 컴파일러가 변수의 타입을 스스로 찾아낼 수 있도록 하는 기능입니다.

Java 9까지는 변수의 타입을 명시적으로 표현하고 초기화에 사용된 이니셜 라이저(초기화 값)와 호환되는지 확인했었습니다.

```java
String msg = "Hello Java";
```

Java 10에서 `var`라는 Local Variable Type-Inference이 추가 되었고 다음과 같이 선언할 수 있습니다. 

```java
@Test
public void isVarTypeString() {
    var msg = "Hello Java";
    assertTrue(msg, instanceof String);
}
```

위의 내용을 보면 변수 msg의 타입을 선언하지 않았습니다. 대신 `var`로 표현하고 **컴파일러가 이니셜 라이저의 유형에서 변수 msg의 타입을 추론**하게 됩니다.

참고로 **타입 추론은 이니셜 라이저(초기화 값)이 있는 지역 변수에만 사용할 수 있습니다.** 멤버 변수, 메소드 매개 변수, 반환 타입 등에는 사용할 수 없습니다.

```java
var num; // error: 초기화 필요
var num = null; // error: variable initializer is 'null'
public var num = "hello"; // error: 멤버 변수에 사용 불가
public void func(var num){} // error: 메소드 매개 변수에 사용 불가
public var func() {} // error: 반환 타입에 사용 불가
var str = (String s) -> s.length() > 10; // error: Lambda 표현식에는 명시적인 대상이 필요합니다.
var arr = {1,2,3}; // error: 배열 이니셜 라이저 사용 불가
```

타입 추론은 아래와 같이 코드를 줄이는 데 도움이됩니다.

```java
// before
Hash<Integer, String> map = new HashMap<>();
// after
var map = new HashMap<Integer, String>();
```

그리고 `var` 은 키워드가 아니라 byte, int와 같은 **예약어**입니다. 또한, `var`의 변수 유형은 **컴파일 시점에 유추**되기 때문에 `var` 변수 사용시 **런타임 오버 헤드가 없습니다.**

#### var 사용 유의사항
첫 번째, **반환 타입을 이해하기 어려운 상황**

```java
var result = obj.process();
```

두 번째, **파이프 라인이 긴 스트림**

```java
var result = arr.stream()
    .findFirst()
    .map(String::length)
    .orElse(0);
```

세 번쨰, **Java 7에 도입된 다이아몬드 연산자(<>)**

```java
var result = new HashMap<>(); // 이 경우 empty Map이 됩니다.
var result = new HashMap<Integer, String>(); // <>안에 Object를 명시적으로 표현
```

위와 같이 **Map의 <>안에 Object를 명시적으로 표현해야 합니다.**

## References
* [live-study-week-2](https://github.com/whiteship/live-study/issues/2)
* 뇌를 자극하는 Java 프로그래밍
* [kephilab.tistory](https://kephilab.tistory.com/27)
* [java-10-local-variable-type-inference](https://www.baeldung.com/java-10-local-variable-type-inference)
* [Local Variable Type Inference: Style Guidelines](https://openjdk.java.net/projects/amber/LVTIstyle.html)