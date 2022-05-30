---
title: 연산자
category: Java
date: 2022-05-30 00:30:59
lastmod: 2022-05-30 00:30:59
comments: true
order: 17
---

## 목표
자바가 제공하는 다양한 연산자를 학습하세요.

## 학습할 것
* 산술 연산자
* 비트 연산자
* 관계(비교) 연산자
* 논리 연산자
* instanceof
* assignment(=) operator
* 화살표(->) 연산자
* 3항 연산자
* 연산자 우선 순위
* (optional) Java 13. switch 연산자


## 산술 연산자
* **"+"**: 더하기
* **"-"**: 빼기
* **"*"**: 곱하기
* **"/"**: 나누기(몫)
* **"%"**: 나누기(나머지)

## 비트 연산자
* **"&"**: AND
* **"\|"**: OR
* **"^"**: XOR
* **"~"**: NOT

```java
class Main {
    public static void main(String args[]) {
        int A = 2;// 0010
        int B = 3;// 0011
        System.out.println(A&B);//2 : 0010
        System.out.println(A|B);//3 : 0011
        System.out.println(A^B);//1 : 0001
        System.out.println(~A);//-3 : (1)0011 (보수)
    }
}
```

## 관계(비교) 연산자
* **">"**: A > B
* **"<"**: A < B
* **">="**: A >= B
* **"<="**: A <= B
* **"=="**: A == B
* **"!="**: A != B

## 논리 연산자
* **"&"**: AND
* **"\|"**: OR
* **"^"**: XOR
* **"~"**: NOT

## instanceof
**객체 타입**을 확인하는데 사용하는 연산자, 반환 타입은 True, False 입니다.

```java
public static class A {
    //...
}

public static class B extends A {
    //...
}

class Main {
    public static void main(String args[]) {
        A a = new A();
        B b = new B();
        // 인스턴스 + instanceof + 클래스
        System.out.println((a instanceof A));// true
        System.out.println((a instanceof B));// false
        System.out.println((b instanceof B));// true
        System.out.println((b instanceof A));// true
    }
}
```

## assignment(=) operator
* **"+="**: a += b(a = a + b)
* **"-="**: a -= b(a = a - b)
* **"*="**: a *= b(a = a * b)
* **"/="**: a /= b(a = a / b)
* **"%="**: a %= b(a = a % b)
* **"&="**: a &= b(a = a & b)
* **"\|="**: a \|= b(a = a \| b)
* **"^="**: a ^= b(a = a + b)
* **"\>>="**: a \>>= b(a = a \>> b)
* **"\<<="**: a \<<= b(a = a \<< b)
* **"\>>>="**: a \>>>= b(a = a \>>> b)
  * **"\>>>"**는 지정한 수만큼 비트를 전부 오른쪽으로 이동시키며, 새로운 비트는 전부 0으로 채웁니다.


## 화살표(->) 연산자
화살표(->) 연산자는 **람다식**을 의미합니다. 람다식이란? 별도의 식별자없이 실행가능한 함수를 의미합니다.

```java
class Car {
    int fuel;
    Car(int fuel) {
        this.fuel = fuel;
    }

    public void drive(int distance) {
        this.fuel -= distance;
    }

    public void fillFuel(int fuel) {
        this.fuel += fuel;
    }
}

class Main {
    public static void main(String args[]) {
        Car car = new Car(10);
        car.drive(3);
        car.fillFuel(10);
    }
}
```

## 3항 연산자
**조건 ? 참인 경우 : 거짓인 경우**

```java
public class Main {
    public static void main(String[] args) {
        String message1 = "";
        int input = 6;
        if (input > 5) {
            message1 = "hello";
        } else {
            message1 = "bye";
        }

        // 3항 연산자
        String message2 = (input > 5) ? "hello" : "bye";
        System.out.println("message1 = " + message1); // hello
        System.out.println("message2 = " + message2); // hello
    }
}
```

## 연산자 우선 순위

|우선 순위|연산자|
|:---:|:---:|
|1|(), []|
|2|!, ~, ++, --|
|3|*, /, %|
|4|+, -|
|5|\<<, \>>, \>>>|
|6|<, <=, >, >=|
|7|==, !=|
|8|&|
|9|^|
|10|\||
|11|&&|
|12|\|\||
|13|? :|
|14|=, +=, -=, *=, /=, \<<=, \>>=, &=, ^=, ~=|


## (optional) Java 13. switch 연산자
switch 연산자는 **Java 12** 부터 추가된 연산자입니다.(기존 switch case 구문이 변경된 것이 아닙니다.)
기존 switch 구문은 **구문(statement)** 이라면 Java 13 부터는 **연산자(operator or expression)** 라고 합니다. 

Java 12, 13에서의 주요 특징은 다음과 같습니다.

#### Java 12 
* 콤마(,)를 사용하여 여러 case를 표현
* value break를 사용하지 않고 화살표(arrow, ->)를 사용하여 결과 반환

```java
private static int getDayArrow(String day) {
    int result = switch (day) {
        case "mon", "tue" -> 1;
        case "wed" -> 2;
        case "thu", "sat", "sun" -> {
            System.out.println("Supports multi line block!");
            break 3;
        }
        default -> -1;
    };
    return result;
}
```

#### Java 13
* Java 12 에서의 value break 구문 대신 **yield 키워드** 사용하여 결과 반환

```java
private static int getDayYield(String day) {
    int result = switch (day) {
        case "mon", "tue":
            yield 1;
        case "thu":
            yield 2;
        case "wed", "sat", "sun":
            System.out.println("Supports multi line block!");
            yield 3;
        default:
            yield -1;
    };
    return result;
}
```

## References
* [live-study-week-3](https://github.com/whiteship/live-study/issues/3)
* [coding-factory.tistory](https://coding-factory.tistory.com/265)
* [tcpschool: lambda](http://www.tcpschool.com/java/java_lambda_concept)
* [java 13. switch expressions](https://openjdk.java.net/jeps/354)
