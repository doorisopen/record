---
title: "Comparable와 Comparator"
category: Java
date: 2020-07-11 00:30:59
comments: true
order: 11
---


> Comparable와 Comparator을 이용하여 Java 객체를 정렬할 수 있습니다.

## Comparable(interface)
* java.lang.Comparable
* 정렬 수행 시 기본적으로 적용되는 정렬 기준이 되는 메서드를 정의하는 인터페이스
  + 즉, Java에서 제공되는 정렬이 가능한 클래스들은 모두 Comparable 인터페이스를 구현하고 있습니다.
  + Integer, Double Class : 오름차순(default)
  + String Class : 사전순(default)

#### Comparable 구현 방법
* 정렬할 객체에 Comparable interface를 implements 후, compareTo() 메서드를 오버라이드하여 구현한다.

```java
class Info implements Comparable<Info> {
    int cost;
    int x, y;
    public Info(int c, int x, int y) {
        this.cost = c;
        this.x = x;
        this.y = y;
    }

    @Override
    public int compareTo(Info info) {
        return info.cost >= this.cost ? 1 : -1;
    }
}
```

#### @Override compareTo()
* compareTo() 메서드 원리
  + 현재 객체 < 파라미터로 넘어온 객체: 음수 return
  + 현재 객체 == 파라미터로 넘어온 객체: 0 return
  + 현재 객체 > 파라미터로 넘어온 객체: 양수 return
  + 음수 또는 0이면 객체의 자리가 유지되며, 양수인 경우에는 두 객체의 자리가 바뀌는 것입니다.
  + 즉, 작거나 0이면 객체의 자리가 유지되기 때문에 오름차순으로 구현이 되는 것입니다.


## ※참고※ Arrays.sort()와 Collections.sort()의 차이
#### Arrays.sort()
* __배열__ 정렬의 경우 사용합니다.
* Ex) byte[], char[], double[], int[], Object[], T[] 등, __Object Array__ 에서는 `TimSort`(Merge Sort + Insertion Sort)를 사용
* Object Array: 새로 정의한 클래스에 대한 배열, __Primitive Array__ 에서는 `Dual Pivot QuickSort`(Quick Sort + Insertion Sort)를 사용
* Primitive Array: 기본 자료형에 대한 배열

#### Collections.sort()
* List Collection 정렬의 경우
* Ex) ArrayList, LinkedList, Vector 등, 내부적으로 Arrays.sort()를 사용

## Comparator(interface)
* java.util.Comparator
* 정렬 가능한 클래스(Comparable 인터페이스를 구현한 클래스)들의 __기본 정렬 기준과 다르게 정렬 하고 싶을 때 사용__ 하는 인터페이스입니다.
* 주로 익명 클래스로 사용된다.
* 기본적인 정렬 방법인 오름차순 정렬을 __내림차순으로 정렬할 때 많이 사용합니다.__

#### Comparator 구현 방법
* Comparator interface를 implements 후 compare() 메서드를 오버라이드한 myComparator class를 작성한다.
* compare() 메서드 작성법
  + 첫 번째 파라미터로 넘어온 객체 < 두 번째 파라미터로 넘어온 객체: 음수 return
  + 첫 번째 파라미터로 넘어온 객체 == 두 번째 파라미터로 넘어온 객체: 0 return
  + 첫 번째 파라미터로 넘어온 객체 > 두 번째 파라미터로 넘어온 객체: 양수 return
  + 음수 또는 0이면 객체의 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 변경됩니다.
  + 즉, Integer.compare(x, y)(오름차순 정렬)와 동일한 개념이다.
    - return (x < y) ? -1 : ((x == y) ? 0 : 1);
  + 내림차순 정렬의 경우 두 파라미터의 위치를 바꿔준다.
    - Integer.compare(y, x)(내림차순 정렬)

```java
//문자열 내 마음대로 정렬하기
//https://programmers.co.kr/learn/courses/30/lessons/12915
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public String[] solution(String[] strings, int n) {
        String[] answer = new String[strings.length];
        
        Comparator<String> comparator = new Comparator<String>() {
            public int compare(String s1, String s2) {
                char c1 = s1.charAt(n);
                char c2 = s2.charAt(n);
               
                if(c1 > c2) return 1;
                else if(c1 < c2) return -1;
                else return 0;
            }
        };
        Arrays.sort(strings);
        Arrays.sort(strings, comparator);
        //...(생략)...
    }
}
```

## References
* [gmlwjd9405.github.io](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)
* [programmers/문자열 내 마음대로 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/12915)