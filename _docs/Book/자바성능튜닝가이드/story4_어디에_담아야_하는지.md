# story4. 어디에 담아야 하는지
생성일: 2022년 6월 12일 오후 1:31

**데이터를 담기 가장 좋은 객체는 `Collection`과 `Map` 인터페이스를 상속한 객체**이다.

![story4-1](/images/Book/자바성능튜닝가이드/story4-1.png)

출처: [https://www.programiz.com/java-programming/collections](https://www.programiz.com/java-programming/collections)

- `Collection`: 가장 상위 인터페이스
- `Set`: 중복을 허용하지 않는 집합을 처리하기 위한 인터페이스
- `SortedSet`: 오름차순을 갖는 Set 인터페이스
- `List`: 순서가 있는 집합을 처리하기 위한 인터페이스
    - 인덱스가 존재하고 위치를 지정하여 값을 찾을 수 있다.
    - 중복을 허용한다.
    - 해당 인터페이스의 구현체 중 가장 많이 사용되는 것은 ArrayList가 있다.
- `Queue`: 여러 개의 객체를 처리하기 전에 담아서 처리할 때 사용하기 위한 인터페이스
    - FIFO(First In First Out)
- `Map`: Map은 키와 값의 쌍으로 구성된 객체의 집합을 처리하기 위한 인터페이스이다.
    - 중복된 키를 허용하지 않는다.
- `SortedMap`: 키를 오름차순으로 정렬한 Map 인터페이스

# Set

- `HashSet`: 데이터를 해쉬 테이블에 담는 클래스로 순서 없이 저장된다.
- `TreeSet`: red-black이라는 트리에 데이터를 담는다. 값에 따라서 순서가 정해진다. 데이터를 담으면서 동시에 정렬을 하기 때문에 HashSet보다 성능상 느리다.
- `LinkedHashSet`: 해쉬 테이블에 데이터를 담는데, 저장된 순서에 따라서 순서가 결정된다.

## 성능 비교

- 저장: add()
    - HashSet >(비슷) LinkedHashSet > TreeSet
    - 참고로 객체의 크기를 미리 지정하고 데이터 삽입을 수행하는 것이 성능상 유리하다.
- 읽기
    - LinkedHashSet >(비슷) HashSet > TreeSet

## 결론

데이터를 순서에 따라 탐색하는 작업이 필요하면 `TreeSet` 을 사용 그 외에는 `HashSet`, `LinkedHashSet`을 사용한다.

# List

- `Vector`: 객체 생성시에 크기를 지정할 필요가 없는 배열 클래스
- `ArrayList`: Vector와 비슷하지만, 동기화 처리가 되어 있지 않다.
- `LinkedList`: ArrayList와 동일하지만, Queue 인터페이스를 구현했기 때문에 FIFO 큐 작업을 수행한다.

## 성능 비교

- 저장
    - ArrayList > Vector > LinkedList
    - 큰 차이가 있는건 아니다.
- 읽기
    - ArrayList > Vector > LinkedList
    - ArrayList가 압도적으로 빠르다.
    - LinkedList를 사용할땐 **get()을 사용하기 보단 peek() 혹은 poll()을 사용하는 것이 성능상 유리**하다.
- 삭제
    - start front: ArrayList >(비슷) LinkedList > Vector
    - start back: ArrayList > LinkedList > Vector
        - 차이가 있긴 하지만 큰 차이는 아닌듯하다.
    - 삭제 연산 시 앞, 뒤에 따라 **ArrayList와 Vector는 차이가 많이 난다.** 그 이유는 앞에서 부터 삭제를 수행하게 되면 첫 번째 데이터 이후(두번째) **데이터들을 앞으로 한 칸씩 옮겨**야하기 때문이다.

### [참고] ArrayList와 Vector

ArrayList와 Vector의 성능 차이가 존재하는 이유는 ArrayList는 여러 스레드에서 접근할 경우 문제가 발생할 수 있지만, Vector는 여러 스레드에서 접근할 경우를 방지하기 위해서 get() 메소드에 synchronized가 선언되어 있다. 따라서 성능 저하가 발생할 수 밖에 없다.

# Map

- `HashTable`: 데이터를 해쉬 테이블에 담는 클래스. 내부에서 관리하는 해쉬 테이블 객체가 동기화되어 있으므로, 동기화가 필요한 부분에서는 해당 클래스를 사용한다.
- `HashMap`: 데이터를 해쉬 테이블에 담는 클래스.
    - HashTable과 차이점
        - NULL 값을 허용
        - 동기화되어 있지 않다.
- `TreeMap`: red-black 트리에 데이터를 담는다.
    - TreeSet과 차이점
        - 키에 의해 순서가 정해진다.
    - 다른 Map에 비해 느린 성능을 가지고 있다.
- `LinkedHashMap`: HashMap과 거의 동일하다
    - 그러나 HashMap과 달리 해당 클래스는 **이중 연결 리스트(doubly-linkedlist)라는 방식을 사용하여 데이터를 담는다.**

# Queue

Queue 인터페이스를 구현한 클래스는 아래 두 가지로 나뉜다.

- java.util
    - LinkedList
    - `PriorityQueue`: 큐에 추가된 순서와 상관없이 먼저 생성된 객체가 먼저 나오도록 되어 있는 큐이다.
- java.util.concurrent(컨커런트 큐 클래스)
    - `LinkedBlockingQueue`: 저장할 데이터의 크기를 선택적으로 정할 수 도 있는 FIFO 기반의 링크 노드를 사용하는 블로킹 큐이다.
    - `ArrayBlockingQueue`: 저장되는 데이터의 크기가 정해져 있는 FIFO 기반의 블로킹 큐
    - `PriorityBlockingQueue`: 저장되는 데이터의 크기가 정해져 있지 않고, 객체의 생성순서에 따라서 순서가 저장되는 블로킹 큐
    - `DelayQueue`: 큐가 대기하는 시간을 지정하여 처리하도록 되어있는 큐
    - `SynchronousQueue`: put() 메소드를 호출하면, 다른 스레드에서 take() 메소드가 호출될 때까지 대기하도록 되어 있는 큐.
        - 해당 큐에는 저장되는 데이터가 없다.
        - API에서 제공하는 대부분의 메소드는 0이나 null을 리턴한다.
        

## List와 Queue 차이점

List는 Queue와 달리 **데이터가 많은 경우 처리 시간이 늘어난다.**

자세히 말하면 **맨 앞 데이터부터 마지막 데이터까지 한 칸씩 옮기는 작업을 수행해야하기 때문에** 데이터가 적을 때는 상관 없지만, 데이터가 많으면 가장 앞에 있는 데이터를 지우는데 소요되는 시간이 증가된다.

# Collection 클래스와 동기화(Synchronized)

- 동기화(Synchronized)된 클래스
    - `Vector`, `HashTable`
- 동기화(Synchronized) 안된 클래스
    - `HashTable`, `TreeSet`, `LinkedHashSet`, `ArrayList`, `LinkedList`, `HashMap`, `TreeMap`, `LinkedHashMap`
- Collections 클래스에는 최신 버전 클래스들의 동기화를 지원하기 위한 synchronized로 시작하는 메소드들을 제공한다

```java
Set s = Collections.synchronizedSet(new HashSet());
...
```

번외로, Map을 사용하는 경우 key 값을 Set(keySet())으로 가져와 iterator를 통해 데이터를 처리하는 경우가 있는데 이때, ConcurrentModificationException 예외가 발생할 수 있다.

해당 예외가 발생하는 여러 원인 중 하나는 스레드에서 iterator로 어떤 Map 객체의 데이터를 꺼내고 있는데, 다른 스레드에서 해당 Map을 수정하는 경우가 있다. 이러한 경우에는 `java.util.concurrent` 패키지에 있는 클래스들에서 해답을 찾을 수 도 있다.