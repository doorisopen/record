---
title: "Java HashSet"
category: Java
date: 2020-02-18 16:40:30
comments: true
order: 6
---

## HashSet 이란 
자바 Collection 중 Set의 대표적인 `HashSet` 클래스를 다루어 보도록 하겠습니다. 

* `HashSet`은 Set의 파생클래스로 Set은 기본적으로 집합이다.
* Set은 null 저장을 허용한다.
* __중복된 원소__ 를 허용하지 않습니다. 
* `HashSet`은 __순서__ 역시 고려가 되지 않습니다.

## Set 인터페이스

|  <center>클래스</center> |   <center>특징</center> |
|:--------:|:--------:|
| __HashSet__ |  __순서가 필요없는 데이터__ 를 hash table에 저장. Set 중에 가장 성능이 좋음 | 
| TreeSet |  저장된 데이터의 값에 따라 정렬됨. red-black tree 타입으로 값이 저장. HashSet보다 성능이 느림 |
| LinkedHashSet |  연결된 목록 타입으로 구현된 hash table에 데이터 저장. 저장된 순서에 따라 값이 정렬. 셋 중 가장 느림 |


## HashSet 예시

```
import java.util.HashSet;

...(중략)...

public static void main(String[] args) {
		HashSet<Integer> set = new HashSet<>();
		
		set.add(1); // 데이터 1 추가
		set.add(2); // 데이터 2 추가
		set.add(3); // 데이터 3 추가
		
		System.out.println(set.size()); // HashSet size : 3

		while(set.iterator().hasNext()) {
			int num = set.iterator().next(); // num: 1 -> 2 -> 3
			set.remove(num); // num 데이터 삭제
			System.out.println(num);
		}

        set.add(4) // 데이터 4 추가
        System.out.println(set.size()); // HashSet size : 1

        set.clear(); // 모든 데이터 삭제
		System.out.println(set.size()); // HashSet size : 0
	}
```