---
title: "OOP(Object Oriented Programming)"
category: Java
date: 2020-10-02 00:30:59
comments: true
order: 13
---


## OOP(Object Oriented Programming)란?
__OOP(Object Oriented Programming - 객체 지향 프로그래밍)__ 는 객체의 관점에서 프로그래밍 하는 것을 의미합니다.

객체 지향 프로그래밍과 반대되는 단어로 __절차 지향 프로그래밍__ 이 있고 C언어가 절차 지향 프로그래밍에 해당합니다. 객체 지향 프로그래밍에는 Java 언어가 있습니다.

## OOP의 특성
#### 추상화(Abstract)
__추상화(Abstract)__ 는 불필요한 정보는 숨기고 중요한 정보만을 표현함으로써 프로그램을 간단히 만드는 기능입니다.

__객체와 클래스 관점__ 에 볼때, 객체들의 공통된 특징을 파악해서 정의해 놓은 개념입니다.

추상화는 __추상(abstract) 혹은 인터페이스(interface) 키워드__ 를 사용하여 공통 특성을 정의할 수 있습니다. 
> 추상(abstract), 인터페이스(interface) 차이점에 대해서 궁금하다면 [이곳](https://doorisopen.github.io/developers-library/Java/2020-06-22-java-abstract-interface/)을 참고해주세요.

```java
abstract public class Human {
	abstract public void sayHello();
}
public class Korea extends Human {
	public void sayHello() {//Human 오버라이드
		System.out.println("안녕하세요");
	}
}

public class Usa extends Human {
	public void sayHello() {//Human 오버라이드
		System.out.println("Hello");
	}
}
public class Main {
	public static void main(String[] args) {
		Korea ko = new Korea();
		Usa us = new Usa();
		ko.sayHello();
		us.sayHello();
	}
}
```

#### 캡슐화(Encapsulation)
__캡슐화(Encapsulation)__ 는 데이터를 외부에 노출시키지 않기 위해 무언가로 한번 감싸는 기능입니다. 이러한 캡슐화는 정보 은닉할 수 있는 특징을 가지고 있습니다.

예를 들어, 스마트폰의 내부에는 다양한 모듈과 회로들이 들어가 있습니다. 그러나 이러한 것들을 외부에 노출 시키지 않기 위해 겉에 케이스를 감싸 캡슐화를 하였다고 할 수 있습니다. 

클래스 관점에서 볼때, 캡슐화는 클래스 혹은 접근 제어자(private)를 활용하여 정보의 노출을 막을 수 있습니다.

#### 상속(Ingeritance)
__상속(Ingeritance)__ 은 새로운 클래스가 기존의 클래스의 자료와 연산을 이용할 수 있게 하는 기능입니다.

예를 들어, 독수리, 참새, 까치 클래스가 있다고 할때 이들은 조류라는 공통적 속성을 가지고 있음을 알 수 있습니다. 따라서, 이러한 공통 속성을 조류라는 클래스에 정의하고 독수리, 참새, 까치 클래스가 조류 클래스를 상속받아 조류 클래스에 정의된 속성들을 물려준다고 생각하면 됩니다.

상속이 필요한 이유는 위의 예시 처럼 __코드의 중복을 없애기 위함__ 입니다. 코드의 중복이 많아지면 유지 보수에서 어려움이 발생할 수 있습니다.(코드의 이원화 문제)

```java
public class 조류 {
	...
}
public class 독수리 extends 조류 {
	...
}
```

#### 다형성(Polymorphism)
__다형성(Polymorphism)__ 은 서로 다른 클래스로부터 만들어진 객체지만 같은 부모의 Class 타입으로 이들을 관리할 수 있는(=대입될 수 있는) 기능입니다.

클래스 관점에서 볼때, 자식 클래스는 부모 클래스에 정의된 메서드 등을 __오버라이딩__(같은 이름의 메소드가 여러 클래스에서 다른 기능을 하는 것)이나 __오버로딩__(같은 이름의 메소드가 인자의 개수나 자료형에 따라서 다른 기능을 하는 것)을 의미합니다.

```java
public class Language {
	public String sayHello() {
		return "sayHello()";
	}
}
public class Korea extends Language {
	public String sayHello() {//부모 sayHello() 오버라이드
		return "안녕하세요";
	}
}
public class Usa extends Language {
	public String sayHello() {
		return "Hello";
	}
}
public class Main {
	public static void main(String[] args) {
		Language ko = new Korea();// ko는 Korea객체를 인스턴스화 했지만 Language 클래스(부모) 행세를 합니다.
		Language us = new Usa();

		System.out.println(ko.sayHello());//안녕하세요
		System.out.println(us.sayHello());//Hello
	}
}
```


## Reference
* [wikipedia.org](https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
* [victorydntmd.tistory.com/117](https://victorydntmd.tistory.com/117)