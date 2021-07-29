---
title: 디자인 패턴(Design Pattern) 이란
category: DesignPattern
date:   2021-07-29 00:30:59
comments: true
order: 1
---

<center>
    <a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-symbol.png" data-lightbox="falcon9-large" data-title="Check out the image">
        <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-symbol.png" title="Check out the image">
    </a>
</center>

## 목차
* 디자인 패턴 이란?
* 디자인 패턴 종류


## 디자인 패턴 이란?
* 객체 지향 설계를 하다 보면, 이전과 비슷한 상황에서 사용했던 **설계를 재사용**하는 경우가 종종 발생합니다.
* 이러한 설계는 **특정 상황에 맞는 해결책**을 빠르게 찾을 수 있도록 도와줍니다.
* 반복적으로 사용되는 설계는 클래스, 객체의 구성, 객체 간 메시지 흐름에서 **일정 패턴을 갖습니다.**
* **상황에 맞는 올바른 설계**를 더 빠르게 적용할 수 있습니다.
* 각 패턴의 장단점을 통해서 설계를 선택하는데 도움을 얻을 수 있습니다.
* 설계 패턴에 이름을 붙임으로써 시스템의 문서화, 이해, 유지 보수에 도움을 얻을 수 있습니다.



## 디자인 패턴 종류
GoF 디자인 패턴에서 디자인 패턴을 다음 2가지 기준으로 분류합니다.

* **목적** : 생성, 구조, 행위
  + **생성:** 객체의 생성 과정에 관련한 패턴
  + **구조:** 객체의 합성과 관련한 패턴
  + **행위:** 객체 간 상호작용과 관련한 패턴

* **범위** : 클래스, 객체
  + **클래스:** 클래스와 서브클래스 간의 관련성을 다룹니다. 주로 상속과 관련 돼있고, 컴파일 시점에 정적으로 결정됩니다.
  + **객체:** 객체 간의 상호작용과 관련해서 다룹니다. 런타임 시점에 변경되는 동적인 성격을 가지고 있습니다.


<a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-intro-category.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-intro-category.png" title="Check out the image">
</a>


## References
* 개발자가 반드시 정복해야할 객체지향과 디자인 패턴 - 최범균