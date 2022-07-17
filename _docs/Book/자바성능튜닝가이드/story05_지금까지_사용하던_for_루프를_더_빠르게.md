# story5. 지금까지 사용하던 for 루프를 더 빠르게 할 수 있다고?
생성일: 2022년 6월 15일 오후 9:42

---

- ~JDK 6: switch-case문에서는 주로 **정수와 enum**로만 처리
- JDK 7~: String을 비교 가능
    - Object 클래스에 선언되어있는 **hashCode() 메소드**를 통해서 비교한다.
    - String에서 Overriding한 hashCode() 메소드에서 문자열을 int 값으로 구분하여 사용한다.
    - switch-case 사용 시, 변환한 값들은 오름차순으로 정렬하고 equals() 메소드로 비교한다.
- **switch-case 문**은 작은 숫자부터 큰 숫자를 비교하는 것이 가장 빠르다.

JDK 5 이전에는 for 문을 사용할 때 아래와 같이 사용했다.

```java
List<Integer> list = new ArrayList<>();
for (int idx = 0; idx < list.size(); idx++) {
}
```

그러나, 위 방식은 좋은 습관이 아니다. 이유는 list.size()를 호출하기 때문이다. 

그래서 다음과 같이 수정하여 사용하는 것이 좋다.

```java
List<Integer> list = new ArrayList<>();
int size = list.size()
for (int idx = 0; idx < size; idx++) {
}
```

JDK 5 부터는 **for-each 라고 불리는 for 루프**를 사용할 수 있습니다.

```java
List<Integer> list = new ArrayList<>();
for (int item : list) {
}
```

for-each를 사용하면 별도의 형 변환이나 list.get() 메소드를 호출할 필요가 없다.

그러나, 위 방법은 첫 인덱스 부터 리스트 마지막 인덱스를 순회하고 특정 인덱스를 지정해서 조회할 수 없다는 단점이 있다.

## 그외..

향상된 for문..