---
title: JAVA로 알고리즘 시작하기 2편
category: Algorithm
comments: true
order: 3
---

## 알고리즘 시작하기 JAVA 2편
Java로 PS(Problem Solving)을 시작할때 필수로 알아야할 기본기를 정리하였습니다. 목차는 아래와 같습니다.

* Java의 자료구조(java.util)
  + List
    - ArrayList, LinkedList, (Vector)
  + Stack
    - LinkedList, (Stack)
  + Queue
    - LinkedList
  + Hash Table
    - HashMap, (HashTable)
  + Set
    - HashSet

## 리스트(List)
* 데이터를 1차원으로 늘어놓은 형태의 __자료구조__ 를 말합니다.
* 1차원 형태로 데이터를 저장하는 방법으로는 배열도 있지만, __리스트는 배열과 달리 데이터의 검색, 추가, 삭제가 가능__ 합니다.
* 리스트는 데이터를 저장하는 형태에 따라 __배열 리스트(array list)와 연결 리스트(linked list)로 세분화__ 합니다.

### ArrayList와 LinkedList
![algorithm-start_algorithm_java_2_1](/images/Algorithm/algorithm-start_algorithm_java_2_1.JPG)

|  <center>종류</center> |  <center>추가/삭제</center> |  <center>인덱스 조회</center> |
|:--------:|:--------:|:--------:|
|Array List|느림|빠름|
|Linked List|빠름|느림|

* __데이터 검색__ 시에는 ArrayList는 LinkedList에 비해 굉장히 빠르다. ArrayList는 인덱스 기반의 자료 구조이며 get(int index) 를 통해 O(1) 의 시간 복잡도를 가진다. 그에 비해 LinkedList는 검색 시 모든 요소를 탐색해야 하기 때문에 최악의 경우에는 O(N)의 시간 복잡도를 가진다.
* LinkedList에서의 __데이터의 삽입, 삭제__ 시에는 ArrayList와 비교해 굉장히 빠른데, LinkedList는 이전 노드와 다음 노드를 참조하는 상태만 변경하면 되기 때문이다. 삽입, 삭제가 일어날 때 O(1)의 시작 복잡도를 가진다. 반면 ArrayList의 경우 삽입, 삭제 이후 다른 데이터를 복사해야 하기 때문에 최악의 경우 O(N) 의 성능을 내게 된다.

### ArrayList
* java.util.ArrayList;

```
import java.util.ArrayList;

ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(10);// 10
numbers.add(20);// 10 20
numbers.add(30);// 10 20 30
numbers.add(40);// 10 20 30 40

numbers.add(1, 50);// 10 50 20 30 40
numbers.remove(2);// 10 50 30 40
numbers.get(2);// 30

//Array List 순회(반복)1
Iterator it<Integer> = numbers.iterator();
while(it.hasNext()){
    System.out.println(it.next());          
}

//Array List 순회(반복)2
for(int value : numbers){
    System.out.println(value);
}
```

### LinkedList
* java.util.LinkedList

```
LinkedList<Integer> linkedList1 = new LinkedList<Integer>();
LinkedList<Integer> linkedList2 = new LinkedList<Integer>();
Object obj = new Object();

linkedList1.add(1);// linkedList1: [1]
linkedList1.add(2);// linkedList1: [1, 2]
linkedList1.add(3);// linkedList1: [1, 2, 3]
System.out.println(linkedList1);// [1, 2, 3] 출력

linkedList2.addAll(linkedList1);// linkedList2: [1, 2, 3]

linkedList2.addFirst(100);// linkedList2 제일 처음에 추가: [100, 1, 2, 3]
linkedList2.addLast(1000);// linkedList2 제일 마지막에 추가: [100, 1, 2, 3, 1000]
System.out.println(linkedList2);// [100, 1, 2, 3, 1000] 출력

// linkedList2의 0번째 있는 요소를 반환
System.out.println(linkedList2.get(0));// 100 출력

obj = linkedList1.clone();// 링크드 리스트 객체의 단순 복사본을 반환
System.out.println(obj);// [1, 2, 3] 출력

// 리스트 안에 있는 데이터중 찾고자 하는 값이 존재하는지 확인 할 수 있는 함수
// 인자로 object를 넘기면 boolean을 리턴
System.out.println(linkedList2.contains(3));// true 출력

linkedList2.sort(null);//오름차순
System.out.println(linkedList2);// [1, 2, 3, 100, 1000] 출력
```

* `removeFirst()` : 첫 번째 요소 제거
* `removeLast()` : 마지막 요소 제거
* `set(idx, data)` : idx번째 요소를 data으로 설정한다.
* `sort(null)` : 오름차순으로 정렬
* `link1.retainAll(link2)` : link2에 있는 요소와 link1과 중복 요소만 출력.(중복 제거)
* `push(data)` : 맨앞에 data 삽입 ex) link.push(20); -> [20, 10, 30, 40]
* `link.pop()` : 맨 앞 요소 빼냄 ex) link.pop(); -> [10, 30, 40]
* `subList(start, end)` : 리스트에서 start부터 end-1까지 출력
* `peek()` : 첫 요소 리턴
* `peekFirst()` : 첫 요소 리턴, 리스트가 비어있으면 null 반환
* `poll()` : 첫 요소 리턴 후 삭제
* `pollFirst()` : 첫 요소를 가져 온 후 삭제함, 리스트 비어있으면 null 반환
* `pollLast()` : 마지막 요소를 가져 온 후 삭제함, 리스트 비어있으면 null 반환
* `offer(data)` : 맨 마지막에 data 요소 추가
* `offerLast(data)` : 맨 마지막에 data 요소 추가
* `offerFirst(data)` : 맨 앞에 요소 추가

## Stack
* java.util.Stack;

```
Stack<Integer> stack = new Stack<>();
stack.push(3);
stack.push(2);
stack.push(6);
stack.push(4);

System.out.println(stack.top());// 4

stack.pop(); // 4 제거

System.out.println(stack.size());// 3
System.out.println(stack.peek());//데이터를 빼지 않고 현재 가장 위에 위치하는 데이터 확인
```

## Queue
* java.util.Queue;

```
Queue<Integer> queue = new LinkedList<Integer>();

q.offer(2);
q.offer(3);
q.offer(1);
System.out.println(q.peek()); // 2
System.out.println(q.poll()); // 2반환 후 제거
System.out.println(q.peek()); // 3
q.remove(3);// 3 제거
q.offer(5); // 1 5
q.offer(6); // 1 5 6
q.remove(5);// 1 6
System.out.println(q.peek());// 1
```

## HashTable
* 해쉬 테이블은 여러 개의 통에 번호를 붙여놓고, 키 값을 이용하여 데이터가 들어갈 통 번호를 계산하는 자료구조입니다.

### HashMap
* java.lang.Object

```
HashMap<String, Integer> hashtable = new HashMap<String, Integer>();
hashtable.put("A", 90);
hashtable.put("B", 80);
hashtable.put("C", 50);
hashtable.put("D", 30);

System.out.println(hashtable.get("A")); // 90
hashtable.remove("B");
```

## Set
### HashSet

```
HashSet<String> set = new HashSet<String>();
set.add("apple");
set.add("orange");
set.add("banana");

System.out.println(set.size());//3
set.remove("orange");
System.out.println(set.size());//2

Iterator<String> it = set.iterator();
while(it.hasNext()) {
    System.out.println(it.next());
}
```

# References
* 뇌를 자극하는 Java 프로그래밍
* [https://developer.android.com/reference/java/util/LinkedList](https://developer.android.com/reference/java/util/LinkedList)
* [http://tcpschool.com/java/java_collectionFramework_stackQueue](http://tcpschool.com/java/java_collectionFramework_stackQueue)