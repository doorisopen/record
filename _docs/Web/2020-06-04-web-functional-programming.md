---
title: 함수형 프로그래밍이란?
category: Web
date:   2020-06-04 00:30:59
comments: true
order: 7
---

## 함수형 프로그래밍
* 수학에서의 함수로 프로그래밍을 한다고 생각하면 된다.
* 종류
  + Lisp 계열 : Common Lisp, Scheme, Clojure,...
  + ML 계열 : OCaml, Standard ML, F#,...
  + Scala
  + Erlang 계열(Erlang, Elixir,...)
  + Haskell

## 왜 함수형 프로그래밍 언어 인가??
* 높은 표현력을 통해 불필요한 코드를 줄일 수 있다.
* 함수형 프로그래밍 언어군은 프로그래밍 언어론의 최신 연구 결과를 반영하고있다.
  + 소프트웨어 트랜잭션 메모리, 타입 클래스, 대수적 자료형과 패털 매칭 등

## 자바스크립트는 함수형 프로그래밍 언어가 아니다
함수형 언어는 여러분이 부작용을 제거할 수 있는 곳에서는 제거를 돕고, 그럴수 없는 곳에서는 통제할 수 있게 도와준다. 자바스크립트는 이 기준을 만족하지 않는다. 사실 자바스크립트가 적극적으로 부작용을 장려하는 것은 쉽게 찾을 수 있다.
가장 쉽게 찾을 수 있는 것이 바로 this이다. this는 모든 함수의 숨겨진 입력이다. 특히 this가 마법처럼 보이는 까닭은 의미 변화가 자유롭기 때문이다. 심지어 전문가라고 할만한 자바스크립트 프로그래머들조차 this의 참조 대상을 추적하는데 어려움을 겪는다. 함수형 관점에서 보자면 어디서나 마법처럼 접근 가능하다는 사실이 설계 결함의 징후(design smell)이다.
여러분은 분명 FP 라이브러리(예를 들어 Immutable.js)를 로드할 수 있고 또 이를 통해 함수형 스타일로 프로그래밍 하기가 더 쉬워지기는 하겠지만 언어 자체의 본질은 바뀌지 않는다.


## 함수형 프로그래밍 요소
* 고계 함수
* 일급 함수
* 커링과 부분 적용
* 재귀와 꼬리 재귀 최적화


* 멱등성
  + 같은 입력에 대해서 같은 출력이 나온다.
* 순수 함수와 참조 투명성
* 불변성과 영속적 자료구조, 메모이제이션

#### 일급 함수
함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 표현합니다. 예를 들어, 일급 함수를 가진 언어에서는 __함수를 다른 함수에 매개변수로 제공하거나, 함수가 함수를 반환할 수 있으며, 변수에도 할당할 수 있습니다.__

##### 일급 함수 예시
* 변수에 함수 할당
* 함수를 인자로 전달(콜백 함수)
* 함수 반환(고차 함수)
  + 변수 사용
  + 이중 괄호 사용


```javascript
//==ex1 | 변수에 함수 할당==//
const foo = function() {
   console.log("foobar");
}
// 변수를 사용해 호출
foo();
```

* 익명함수를 변수에 할당한 다음, 그 변수를 사용하여 끝에 괄호 ()를 추가하여 함수를 호출했습니다.
* __함수가 이름을 가지고 있더라도__ 할당한 변수 이름을 사용해 함수를 호출할 수 있습니다. 이름을 지정하면 코드를 디버깅할 때 유용합니다. 하지만 호출하는 방식에는 영향을 미치지 않습니다.

<hr/>

```javascript
//==ex2 | 함수를 인자로 전달==//
function sayHello() {
   return "Hello, ";
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
// `sayHello`를 `greeting` 함수에 인자로 전달
greeting(sayHello, "JavaScript!");
```

* sayHello() 함수를 greeting() 함수의 인자로 전달했습니다. 이것이 함수를 어떻게 변수처럼 다루는지 보여주는 예시입니다.
* 다른 함수에 인자로 전달된 함수를 콜백 함수라고 합니다. sayHello는 콜백 함수입니다.

<hr/>

```javascript
//==ex3 | 함수 반환==//
function sayHello() {
   return function() {
      console.log("Hello!");
   }
}
```

* JavaScript에서는 함수를 변수처럼 취급하기 때문에 함수를 반환할 수 있습니다.
* 함수를 반환하는 함수를 __고차 함수__ 라고 부릅니다.

```javascript
//==ex3 | 함수 반환 - 변수 사용==//
const sayHello = function() {
   return function() {
      console.log("Hello!");
   }
}
const myFunc = sayHello();
myFunc(); //Hello! 출력
```

* 만약에 sayHello 함수를 직접 호출하면, 반환된 함수를 호출하지않고 함수 자체를 반환합니다. 그러므로 반환된 함수를 다른 변수에 저장하여 사용해야합니다.

```javascript
//==ex3 | 함수 반환 - 이중 괄호 사용==//
function sayHello() {
   return function() {
      console.log("Hello!");
   }
}
sayHello()();
```

* 이중 괄호 ()()를 사용해 반환한 함수도 호출하고 있습니다.

## References
* [어떤-프로그래밍-언어들이-함수형인가](https://medium.com/@jooyunghan/%EC%96%B4%EB%96%A4-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%96%B8%EC%96%B4%EB%93%A4%EC%9D%B4-%ED%95%A8%EC%88%98%ED%98%95%EC%9D%B8%EA%B0%80-fec1e941c47f)
* [[10분 테코톡]도넛의 함수형 프로그래밍](https://www.youtube.com/watch?v=ii5hnSCE6No)
* [developer.mozilla.org](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)