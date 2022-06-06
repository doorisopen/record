---
title: 반복문,조건문
category: Java
date: 2022-06-06 00:30:59
lastmod: 2022-06-06 00:30:59
comments: true
order: 18
---

## 목표
자바가 제공하는 제어문을 학습하세요.

## 학습할 것
* 선택문
* 반복문

## 제어문
> **제어문(control flow statements)**은 프로그램의 흐름을 제어하는 경우에 사용하는 실행문으로, **조건문, 반복문, 분기문** 등이 포함되어 있다.

| 제어문 종류 | 선택 제어 | 반복 제어 | 무조건 분기 |
|:---:|:---:|:---:|:---:|
| 제어문 | if 문 <br> if ~ else 문 <br> if ~ else if ~ else 문 <br> if ~ (if ~ else) ~ else 문 <br> switch ~ case 문 | for 문 <br> while 문 <br> do ~ while | goto 문|
| 기타 제어문 | break 문 <br> continue 문 || return 문 |


## 선택문
`if 문` 은 주어진 조건이(isStudent)이 참인 경우 하위 블록 내용이 수행되는 구문입니다.

```java
if (isStudent()) {
    ...
}
```

`if ~ else 문` 은 주어진 조건이(isStudent)이 참인 경우 if 블록 내용이 수행되고 거짓인 경우 else 블록이 수행되는 구문입니다.

```java
if (isStudent()) {
    ...
} else {
    ...
}
```

`if ~ else if ~ else 문` 은 다중 조건을 검사할 수 있는 구문으로 최상위 if 의 조건 부터 차례로 위에서 아래로 검사하고 참인 경우 해당 블록이 수행되고 종료합니다. 만약 참인 조건이 존재하지 않는다면 else 블록을 수행하고 종료합니다.

```java
if (isStudent()) {
    ...
} else if (isTeacher()) {
    ...
} else {
    ...
}
```

`if ~ (if ~ else) ~ else 문` 은 내부에 중첩된 구조로 외부에 존재하는 if (isStudent())의 조건이 참인 경우 해당 구문의 블록의 if (isJunior()) 구문을 수행합니다.

```java
if (isStudent()) {
    if (isJunior()) {
        ...
    } else {
        ...
    }
} else {
    ...
}
```

`switch ~ case 문` 주어진 조건식의 값에 따라 여러 개의 명령문 중에서 어느 특정한 명령문만을 실행 하고자 할 때 사용하는 구문입니다.

```java
switch(grade) {
    case "A":
        ...
        break;
    case "B":
        ...
        break;
    case "C":
        ...
        break;
    case "D":
        ...
        break;
    default:
        break;
}
```

* switch ~ case ~ break(optional) 특정 case에 부합하는 표현식이 있다면 해당 케이스 블록을 수행하고 해당 명령문을 종료합니다.
* 각 case 값(case "A")은 표현식과 동일한 유형이고 **리터럴 혹은 상수** 여야합니다.
* 각 case 값은 고유해야합니다.(중복 케이스가 있으면 컴파일 오류 발생)
* break(optional) 문은 특정 case 수행 후 명령문을 종료하게 합니다.
* break(optional) 문을 제외한 경우, 특정 case 블록 수행 후(break문 X) 다음 break 문에 도달할 때까지 다음 case 블록을 수행합니다.(의도하지 않은 동작 가능성 존재)
* default 문의 경우 특정 case 값과 부합하는 것이 없는 경우 해당 구문을 수행합니다.

## 반복문
반복 제어문은 주어진 조건식의 값을 만족하는 동안 일정한 범위 내의 명령문들을 반복적으로 실행하는 명령문으로 `for 문` `while 문` `do ~ while 문`이 있습니다.

`for 문`

```java
for (초기값; 조건식; 증감값) {
    반복할 명령문
}

for (int count = 0; count < 10; count++) {
    System.out.println(count);
}
```

* **조건식**을 평가하여 참이면 반복할 명령문들을 실행하고 거짓이면 for 문을 벗어납니다.
* **증감값**은 한 번씩 반복할 때마다 변화되는 값을 의미합니다. 
* **초기값과 증감값**은 1개 이상의 식으로 표현이 가능하며 이때 콤마 연산자(,) 사용하여 구분합니다.
* for 문은 형식에서 초기값, 조건식, 증감값 및 반복할 명령문들은 생략이 가능하지만 세미콜론 (;) 은 생략할 수 없습니다.
  * for (;;); 는 아무것도 실행하지 않음
  * for (;;) printf ("A\n"); 는 하나의 문자만을 무한 출력
  

`while 문` 은 **조건식**을 평가하여 조건이 참인 경우에만 반복 실행하고, 조건이 거짓이면 while 문 다음 명령문을 실행합니다(반복문 종료). while 문은 조건이 거짓일 경우 반복할 명령문들을 한 번도 실행하지 않는다.

```java
while(조건식) {
    ...
}
```

`do ~ while 문` 은 while 문과 달리 최소 한 번은 반복문을 수행하고 이후 조건식을 평가하여 다음 반복문을 수행할지를 결정합니다.

```java
do {
    ...
} while(조건식)
```

## 기타
`break 문` 은 반복문 내부에서 외부로 벗어나도록 하여 다음 명령문을 수행하도록 하는 명령문입니다.

```java
for (Student student : students) {
    if (student.isJunior()) {
        break;
    } else {
        System.out.printf("student %s is not junior\n", student.name());
    }
}
//break 이후 수행
System.out.println("bye");
```

`continue 문` 은 반복문 내부에서 일시적으로 일부 명령문을 수행하지 않도록 하는 명령문입니다. (continue 명령문이 수행되면 해당 명령문 이후 부터 반복문 끝까지의 명령문들은 수행되지 않습니다.)

```java
for (Student student : students) {
    if (student.isJunior()) {
        continue;
    }
    System.out.printf("If %s is junior this message not print\n", student.name());
}
```

## 과제 (옵션)
[livestudy project](https://github.com/doorisopen/blog-source-code/tree/master/livestudy)

#### 과제 0. JUnit 5 학습하세요.
* 인텔리J, 이클립스, VS Code에서 JUnit 5로 테스트 코드 작성하는 방법에 익숙해 질 것.
* 이미 JUnit 알고 계신분들은 다른 것 아무거나!
* 더 자바, 테스트 강의도 있으니 참고하세요~

#### 과제 1. live-study 대시 보드를 만드는 코드를 작성하세요.
* 깃헙 이슈 1번부터 18번까지 댓글을 순회하며 댓글을 남긴 사용자를 체크 할 것.
* 참여율을 계산하세요. 총 18회에 중에 몇 %를 참여했는지 소숫점 두자리가지 보여줄 것.
* Github 자바 라이브러리를 사용하면 편리합니다.
* 깃헙 API를 익명으로 호출하는데 제한이 있기 때문에 본인의 깃헙 프로젝트에 이슈를 만들고 테스트를 하시면 더 자주 테스트할 수 있습니다.

#### 과제 2. LinkedList를 구현하세요.
* LinkedList에 대해 공부하세요.
* 정수를 저장하는 ListNode 클래스를 구현하세요.
* ListNode add(ListNode head, ListNode nodeToAdd, int position)를 구현하세요.
* ListNode remove(ListNode head, int positionToRemove)를 구현하세요.
* boolean contains(ListNode head, ListNode nodeTocheck)를 구현하세요.

#### 과제 3. Stack을 구현하세요.
* int 배열을 사용해서 정수를 저장하는 Stack을 구현하세요.
* void push(int data)를 구현하세요.
* int pop()을 구현하세요.

#### 과제 4. 앞서 만든 ListNode를 사용해서 Stack을 구현하세요.
* ListNode head를 가지고 있는 ListNodeStack 클래스를 구현하세요.
* void push(int data)를 구현하세요.
* int pop()을 구현하세요.

#### 과제 5. Queue를 구현하세요.
* 배열을 사용해서 한번
* ListNode를 사용해서 한번.

## References
* [live-study-week-4](https://github.com/whiteship/live-study/issues/4)
* [제어문](http://www.angel25.com/ClanguageStudy5.html)
* [week4 참고 링크1](https://kils-log-of-develop.tistory.com/349#4.-%EA%B8%B0%EB%B3%B8-%EC%A3%BC%EC%84%9D)