---
title: "StringBuffer와 StringBuilder"
category: Java
date: 2020-07-13 00:30:59
comments: true
order: 12
---

## String 클래스
* java.lang.String
* String 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없습니다.
* 이러한 객체를 자바에서는 __불변 객체(immutable object)__ 라고 합니다.
  + 즉, 자바에서 덧셈(+) 연산자를 이용하여 문자열 결합을 수행하면, 기존 문자열의 내용이 변경되는 것이 아니라 내용이 합쳐진 새로운 String 인스턴스가 생성되는 것입니다.


```java
String literalString = "literal"; //리터럴로 생성하는 방식 
String newString = new String("literal"); //new로 생성하는 방식 

//위에서 "literal" 이라는 문자열을 String Pool에서 생성했기 때문에 
//이후에 추가한 str는 추가적으로 생성하지않고 똑같은 문자열을 가리킨다. 
String str = "literal"; 
```

위 코드의 두 가지 String class 생성 방식에 따라 나뉘는데, 두 방식 모두 __JVM 메모리 중 힙(heap) 영역에 생성__ 된다는 점은 같다.

하지만 리터럴로 생성하면 특수하게 __"String Pool"__ 이라는 공간에 생성된다. 이 메모리 공간에 생성된 문자열 값은 절대 변하지 않습니다.(불변 객체)

그래서 '+' 연산이나 .concat() 메소드를 이용해서 문자열 값에 변화를 줘도 메모리 공간 내의 값이 변하는 것이 아니라, "String Pool"이라는 공간 안에 메모리를 할당받아 새로운 String 클래스 객체를 만들어서 문자열을 나타내는 것입니다. 따라서, __String 클래스를 특징__ 을 정리하자면 아래와 같습니다.

* String 클래스는 문자열 연산이 적고, 자주 참조(조회)하는 경우에 사용하면 좋습니다.
* GC(Garbage Collection)의 정리 대상이 될 수 있습니다.
  + [GC 참고 자료](https://doorisopen.github.io/developers-library/Java/2020-02-05-java-garbage-collection/)
* 만약 문자열 연산이 많아진다면, 연산 한 번 할 때마다 계속해서 문자열 객체를 만드는 오버헤드가 발생하므로 성능이 떨어지게 됩니다.(연산에 내부적으로 char배열을 사용함)
* 불변하기 때문에 단순하게 읽어가는 조회연산에서는 타 클래스보다 빠르게 읽을 수 있는 장점이 있습니다. 
* 불변하기 때문에 멀티쓰레드환경에서 동기화를 신경쓸 필요가 없습니다.


## StringBuffer 클래스
* mutable(가변)하다.
* thread-safe하다.
  + StringBuffer는 멀티쓰레드환경에서 synchronized키워드가 가능하므로 __동기화가 가능__ 하기 때문입니다.


## StringBuilder 클래스
* mutable(가변)하다.
* thread-safe하지 않다.
  + StringBuilder는 동기화를 지원하지 않기 때문에 멀티 쓰레드 환경에서는 적합하지 않다.
* StringBuilder가 동기화를 고려하지 않기 때문에 싱글쓰레드 환경에서 StringBuffer에 비해 __연산처리가 빠른 장점__ 이 있다.


## 요약 정리
* String 클래스는 불변하다.
* 문자열 연산이 많은 경우 성능이 안좋음! 조회가 많은 환경, 멀티쓰레드 환경에서 성능적으로 유리하다.
* StringBuffer 클래스와 StringBuilder 클래스는 문자열 연산이 자주 발생할 때 문자열이 변경가능한 객체기 때문에 성능적으로 유리합니다.
* StringBuffer와 StringBuilder의 차이점은 동기화 지원의 유무이다.
* 동기화를 고려하지 않는 환경에서 StringBuilder가 성능이 더 좋고, 동기화가 필요한 멀티쓰레드 환경에서는 StringBuffer를 사용하는 것이 유리합니다.


## References
* [developers-library/java-garbage-collection/](https://doorisopen.github.io/developers-library/Java/2020-02-05-java-garbage-collection/)
* [https://jeong-pro.tistory.com/85](https://jeong-pro.tistory.com/85)
* [http://tcpschool.com/java/java_api_string](http://tcpschool.com/java/java_api_string)