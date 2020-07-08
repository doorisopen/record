---
title: "Java Iterator"
category: Java
date: 2020-02-18 18:39:30
comments: true
order: 7
---

## Iterator 란?
* `Iterator`는 자바의 컬렉션 프레임워크에서 `컬렉션`에 저장되어 있는 __요소들을 읽어오는 방법을 표준화__ 하였는데 그 중 하나가 `Iterator`이다.
* `Vector<E>`, `ArrayList<E>`, `LinkedList<E>`가 상속받는 인터페이스 이다.
* __리스트 구조의 컬렉션__ 에서 요소의 __순차 검색__ 을 위한 메소드를 포함한다.
* 즉, 컬렉션으로부터 쉽게 정보를 얻어내는 인터페이스다.

## Iterator는 인터페이스
```java
public interface Iterator {
    boolean hasNext();
    Object next();
    void remove();
}
```
* `boolean hasNext()` 는 읽어 올 요소가 남아있는지 확인하는 메소드이다. 있으면 true, 없으면 false를 반환한다.
* `Object next()` 는 읽어 올 요소가 남아있는지 확인하는 메소드이다. 있으면 true, 없으면 false를 반환한다.
* `void remove()` 는 next()로 읽어 온 요소를 삭제한다. next() 를 호출한 다음에 remove() 를 호출해야 한다. (선택적 기능이라 사용해도 그만 사용하지 않아도 그만이다)


## Iterator 사용 예시
```java
public static void main(String[] args) {
		ArrayList<Integer> array = new ArrayList<Integer>();
		array.add(1);
		array.add(2);
		array.add(3);
		Iterator<Integer> iter = array.iterator();

		while(iter.hasNext()) {
			System.out.println(iter.next()); / 1 2 3
		}
	}
```



## Iterator VS List size()
`Iterator` 는 자동으로 Index 를 관리해주기 때문에, 사용에 편리함이 있을수 있으나

Iterator는 __객체를 만들어 사용하기 때문에__ List size보다 느리다.

그러므로, list 의 size를 받아와서 사용하는 것이 더 좋다.

## 참고 ConcurrentModification Exception
* [ConcurrentModificationException](https://m.blog.naver.com/tmondev/220393974518)

## References
* [Vaert Street](https://vaert.tistory.com/108)