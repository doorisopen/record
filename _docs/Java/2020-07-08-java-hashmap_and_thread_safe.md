---
title: "HashMap과 thread-safe"
category: Java
date: 2020-07-08 00:30:59
comments: true
order: 10
---

이번 포스팅에서는 thread환경에서 HashMap사용에 대해서 알아보겠습니다.


## Thread-Safe 란?
__동기화(Synchronize)__ 라고 표현하기도 하며 어떠한 Class의 인스턴스가 __여러개의 Thread에서 동시 참조되고 해당 객체에 Operation 이 발생해도 정합성을 유지해줄때__ 보통 우리는 Thread-Safe 하다 라고 표현한다.

## HashMap는 thread-safe한가?
결론 부터 말하자면 __HashMap은 thread-safe하지 않다.__ 이유는 여러 스레드가 동일한 HashMap 객체에 액세스하고 HashMap(put(), remove() 메서드) 구조를 수정하려고 하면 HashMap의 상태가 일치하지 않을 수 있기 때문입니다.

HashMap을 멀티 스레드 환경에서 사용하려면 동기화된 블록 내에 동기화(Synchronize)된 블록 내에 관련 코드를 작성하거나 외부 잠금 구현을 사용해야합니다. 그러나 이 경우 적절한 주의를 기울이지 않으면 오류 및 교착 상태 상황이 발생할 가능성이 높습니다.

따라서, HashMap은 멀티 스레드 환경에서는 사용하지 않는 것이 좋습니다. 대신 HashMap과 비슷한 thread-safe한 컬렉션은 다음과 같이 있습니다.

* HashTable
* Collections.SynchronizedMap
* __ConcurrentHashMap__

3개 모두 thread-safe 합니다. 그 중 __ConcurrentHashMap__ 는 나머지 두 개보다 더 나은 성능을 제공합니다.

## HashTable
* HashTable의 thread-safe는 JDK 1.1부터 사용 가능한 레거시 클래스입니다.
* 한 번에 하나의 스레드만 읽거나 쓸 수 있습니다.
  + 즉, 스레드는 전체 HashTable 인스턴스에서 잠금(Lock)이 수반됩니다.
  + 좀 더 설명을 하자면 메서드 호출 전에 쓰레드간 동기화 락(Lock)을 걸기 때문에 멀티 쓰레드(Multi-Thread) 환경에서도 데이터의 무결성을 보장합니다.
  + 이는 __성능 저하의 원인__ 으로 멀티 스레드 아키텍쳐의 장점을 활용할 수 없습니다.

## Collections.SynchronizedMap
* HashMap을 사용하고도 동기화 문제를 해결할 수 있는 방법입니다.
* 어떤 Map 인터페이스(동기화를 지원하지 않는 Map이더라도)를 사용하더라도 SynchronizedMap으로 랩핑(Wrapping)하여 주면 해당 Map객체는 동기화 맵(Synchronized Map)이 됩니다.
* SynchronizedMap은 util 클래스의 static inner class 입니다. (java.util.Collections)
* HashTable 인터페이스를 사용하는 것보다 __더 빠른 처리 속도를 가집니다.__


```java
import java.util.*;
 
public class SynchronizedMapTest {
   public static void main(String[] args) {
    // create HashMap
    Map<String,String> map = new HashMap<>();

    map.put("1","a"); 
    map.put("2","b");
    map.put("3","c");
        
    // create a synchronized map
    Map<String,String> syncmap = Collections.synchronizedMap(map);
        
    System.out.println("Synchronized map is : "+syncmap);
   }
}
```

## ConcurrentHashMap
* JAVA 1.5 부터는 ConcurrentUtil 이라는 인터페이스를 기본으로 제공합니다.
* ConcurrentHashMap의 검색 작업(get 포함)은 Lock이 이루어지지 않으며 갱신 작업(put 및 remove 포함)과 동시에 수행 될 수 있습니다.
* ConcurrentHashMap는 SynchronizedMap 으로 감싸진 HashMap 이나 HashTable 보다 더 빠른 속도를 보입니다.
  + 그 이유는 ConcurrentHashMap은 동기화 시, Map 전체에 동기화 락을 걸지 않고, Map을 여러 조각으로 쪼개어 부분부분 락을 거는 형태로 구현되어 있기 때문입니다. 
* (멀티 쓰레드 환경에서) 쓰레드 간의 경쟁이 심한 경우, 훨씬 더 효율적입니다.
* ConcurrentHashMap의 검색은 검색 method가 실행되는 시점에 __가장 최근에 완료된 갱신 작업의 결과를 반영한다.__

```java
import java.util.concurrent.ConcurrentHashMap;
 
public class ConcurrentHashMapTest {
   public static void main(String[] args) {
    // create ConcurrentHashMap
    Map<String,String> map = new ConcurrentHashMap<>();

    map.put("1","a"); 
    map.put("2","b");
    map.put("3","c");
   }
}
```

## 정리

![java-difference_hashmap_thread_safe]({{ site.baseurl }}/images/Language/Java/java-difference_hashmap_thread_safe.JPG)

## References
* [http://lab.gamecodi.com/](http://lab.gamecodi.com/board/zboard.php?id=GAMECODILAB_QnA_etc&no=3392)
* [https://codepumpkin.com/](https://codepumpkin.com/hashtable-vs-synchronizedmap-vs-concurrenthashmap/)
* [http://blog.breakingthat.com/2019/04/04/java-collection-map-concurrenthashmap/](http://blog.breakingthat.com/2019/04/04/java-collection-map-concurrenthashmap/)
* [https://c10106.tistory.com/1481](https://c10106.tistory.com/1481)
* [이러쿵저러쿵](https://ooz.co.kr/71)