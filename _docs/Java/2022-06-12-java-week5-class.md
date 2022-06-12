---
title: 클래스
category: Java
date: 2022-06-12 00:30:59
lastmod: 2022-06-12 00:30:59
comments: true
order: 19
---

## 목표
자바의 Class에 대해 학습하세요.

## 학습할 것
* 클래스 정의하는 방법
* 객체 만드는 방법 (new 키워드 이해하기)
* 메소드 정의하는 방법
* 생성자 정의하는 방법
* this 키워드 이해하기


## 클래스 정의하는 방법
**클래스(Class)**는 **객체(Object)**를 정의하기 위한 틀입니다.

객체는 **상태**를 가지고 **행위**를 할 수 있습니다. 

* 클래스에서 **상태**는 **필드, 인스턴스 변수, 멤버 변수**라고 정의합니다.
* 클래스에서 **행위**는 **메소드(Method)**라고 정의합니다.

클래스는 아래와 같이 정의할 수 있습니다. 그리고 이는 **Member 객체를 생성**할 수 있는 틀을 정의했다고 할 수 있습니다.

```java
/* class 영역 */
public class Member {//객체
    /* 필드 */
    private static int money = 20;//static 변수(=전역변수)->Method영역

    private String name;//멤버 변수

    /* 생성자 */
    public Member(String name) {
        this.name = name;
    }

    /* 메서드 */
    public void updateMoney() {
        int interest = 5; // 지역 변수->stack영역
        money += interest;
    }
    /* 메서드 */
    public void changeName(String name) {//changeName(매개변수)->stack영역
        this.name = name;
    }
}
```

추가로 클래스는 다음과 같은 Convention으로 정의하는 것을 추천합니다.

```java 
class T {
    상수(static final) 또는 클래스 변수

    인스턴스 변수

    생성자
        부 생성자
        주 생성자

    팩토리 메소드

    메소드

    기본 메소드(equals, hashCode, toString)
}
```

## 객체 만드는 방법 (new 키워드 이해하기)
우리는 위에서 Member 객체를 만들기 위한 클래스라는 틀을 정의했습니다.
그러나 객체를 만들기 위해서는 클래스(Member) 내부에 **생성자(Constructor)** 를 정의해야 객체를 생성할 수 있습니다.

**생성자**는 아래와 같은 형태로 정의할 수 있습니다.

```java
public class Member {//객체
    /* 필드 */
    private String name;//멤버 변수

    /* 생성자 */
    public Member(String name) {//String name -> 파라미터
        this.name = name;
    }

    ...(생략)
```

**생성자**는 클래스에 정의된 상태(필드)들을 입력 값으로 해서 생성할 수 있도록 정의한 것을 말합니다.

예를 들어, 내가 새로운 만화를 구상하고 있는데, 만화 속 등장인물을 만들어야한다고 하자 

이때 등장인물 A에게는 홍길동이라는 이름을 B에게는 김철수라는 이름으로 생성하기 위한 틀을 정의한 거라고 이해하면 됩니다.

이렇게 정의한 생성자는 **new 키워드**를 사용하여 객체를 생성할 수 있습니다.

```java
Member memberA = new Member("홍길동");

Member memberB = new Member("김철수");
```

new 키워드로 생성된 memberA, memberB는 **Member 객체의 인스턴스**라고 불립니다.

여기서 두 관점에서 생각해 볼 점이 있습니다.

* 객체지향
* 시스템(메모리)

우선 **객체지향 관점**에서 이야기하면 객체는 일종의 **타입**이라고 생각하면된다. 객체지향에서 가장 중요한 것은 **클래스가 아닌 객체**라는 점입니다.
또한, 각 객체들은 **상태(위 코드에서는 name)를 가지고 각 상태를 스스로 조정**할 수 있습니다.

**시스템(메모리) 관점**에서 볼 때, 이렇게 생성된 인스턴스들은 메모리 영역 중 **Heap 영역에 적재**되고 **GC에 의해 관리**됩니다.

각 키워드에 대한 자세한 내용은 따로 다루도록 하겠습니다.

## 메소드 정의하는 방법
클래스에서 메소드를 정의한다는 것은 **객체가 자신의 상태를 조절하기 위한 행위를 정의**한다고 할 수 있습니다.

예를 들면, 위에서 등장인물 A의 이름을 홍길동으로 정의했었는데, 홍길동이 본인의 이름이 마음에 들지 않아 "라이언" 이라는 이름으로 개명한다고 하자 이때 홍길동은 다른 사람의 의견에 의해 개명하는 것이 아닌 본인 스스로 개명을 하겠다고 선택한 것이다. 메소드 정의란, 개명을 위한 행위를 정의하는거라고 할 수 있습니다..

```java
public class Member {//객체
    /* 필드 */
    private static int money = 20;//static 변수(=전역변수)->Method영역

    private String name;//멤버 변수

    /* 메서드 */
    public void changeName(String name) {//changeName(매개변수)->stack영역
        this.name = name;
    }
}
```

위와 같이 **changeName()** 메소드를 정의할 수 있고 아래와 같이 사용할 수 있습니다.

```java
Member memberA = new Member("홍길동");

//개명
memberA.changeName("라이언");
```

## 생성자 정의하는 방법
**생성자**는 위에서 이야기 했지만 클래스 내부에서 정의할 수 있습니다.

그리고 생성자는 아래와 같이 다양한 형태로 여러개 정의할 수 있습니다.

참고로 setter 메소드 사용을 지양하고 생성자를 여러개 생성하는 것이 객체지향, 캡슐화 관점에서 권장합니다.

```java
public class Member {//객체
    /* 필드 */
    private String name;//멤버 변수

    /* 생성자 */
    public Member() {
    }

    public Member(String firstName, String lastName) {
        this(firstName + lastName);
    }

    public Member(String name) {//String name -> 파라미터
        this.name = name;
    }

    ...(생략)
```

## this 키워드 이해하기
this 키워드는 **자기 자신을 참조**하고 있는 키워드 입니다. 또한, 클래스 내부에서만 사용될 수 있습니다.

```java
public class Member {//객체
    ...생략

    public void getSelf() {
        System.out.println(this);
    }
}
```

```java
Member member = new Member("홍길동");

// 아래 2개는 동일한 값 출력
System.out.println(member);
System.out.println(member.getSelf());
```

위와 같이 객체 인스턴스와 this를 호출하게 되면 동일한 주소 값을 가지고 있음을 확인할 수 있습니다.

```java
public class Member {//객체
    /* 필드 */
    private String name;//멤버 변수

    public Member(String name) {//String name -> 파라미터
        this.name = name;
    }

    public void getName() {
        return this.name;
    }
```

## 과제 (Optional)
* int 값을 가지고 있는 이진 트리를 나타내는 Node 라는 클래스를 정의하세요.
* int value, Node left, right를 가지고 있어야 합니다.
* BinaryTree라는 클래스를 정의하고 주어진 노드를 기준으로 출력하는 bfs(Node node)와 dfs(Node node) 메소드를 구현하세요.
* DFS는 왼쪽, 루트, 오른쪽 순으로 순회하세요.

[package: assignment6](https://github.com/doorisopen/blog-source-code/tree/master/livestudy/src/main/java)

## References
* [live-study-week-5](https://github.com/whiteship/live-study/issues/5)
