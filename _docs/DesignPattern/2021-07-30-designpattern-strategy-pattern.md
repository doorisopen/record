---
title: 전략 패턴(Strategy Pattern)
category: DesignPattern
date:   2021-07-30 00:30:59
comments: true
order: 3
---

> **※참고※** <br/>
> 디자인 패턴을 소개할때, 특정한 **요구사항**을 구현한 **코드를 제시**하고 해당 코드의 **문제점을 분석**, **패턴을 적용**하는 방식으로 진행합니다.(예제가 적절하지 못할 수 있습니다.)

## 목차
* 요구사항
* 문제점 분석
* 전략 패턴(Strategy Pattern)
  * 패턴 소개
  * 패턴 적용
* 결론

## 요구사항
다음과 같은 요구사항을 구현한 Pos 클래스가 있다고 가정하겠습니다.

* 카페에서 **할인 정책**을 적용하기로 했습니다.
* **"첫 방문 회원"**은 구매한 모든 품목에 500원 할인을 적용한다.
* 커피를 제외한 **"유통기한이 임박한 음식"**의 경우 300원 할인을 적용한다.

```java
public class Pos {

    public int calculate(boolean isFirstMember, List<Item> items) {
        int total = 0;
        for (Item item : items) {
            if (isFirstMember) {
                total += (item.getPrice() - 500);
            } else if (!item.isFresh()) {
                total += (item.getPrice() - 300);
            } else {
                total += item.getPrice();
            }
        }
        return total;
    }
}
```


## 문제점 분석
위 Pos 클래스는 아래와 같은 문제점을 가지고 있습니다.

* (1)의 for문 안에는 **서로 다른 할인 정책들이 한 코드에** 섞여 있습니다.
* 새로운 할인 정책이 추가될 경우 **코드 분석을 어렵**게 만듭니다.
* 새로운 할인 정책이 추가될 때마다 calculate 메소드를 **수정하는 것이 점점 어려워**집니다.

```java
public class Pos {

    public int calculate(boolean isFirstMember, List<Item> items) {
        int total = 0;
        for (Item item : items) { // (1) 문제점 발견!
            if (isFirstMember) {
                total += (item.getPrice() - 500);
            } else if (!item.isFresh()) {
                total += (item.getPrice() - 300);
            } else {
                total += item.getPrice();
            }
        }
        return total;
    }
}
```

앞서 본 문제점을 **전략 패턴**을 적용하는 것으로 문제를 해결할 수 있습니다.


## 전략 패턴(Strategy Pattern)
#### 패턴 소개
* 특정 콘텍스트에서 **알고리즘(할인 전략)을 별도로 분리하는 설계 방법**입니다.
* 콘텍스트는 **사용할 전략을 직접 선택하지 않습니다.**
* 콘텍스트의 **클라이언트가 콘텍스트에 사용할 전략을 전달**해 줍니다. 
  + 즉, DI(의존성 주입)를 이용해서 콘텍스트에 전략을 전달해 줍니다.


<a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-class-diagram.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-class-diagram.png" title="Check out the image">
</a>


#### 패턴 적용
앞서 문제를 가진 코드에 전략패턴을 적용하면 다음과 같습니다. 

<a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-class-diagram-code.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-class-diagram-code.png" title="Check out the image">
</a>

Pos 클래스의 calculate 메소드는 여러 전략들이 섞여있는 것이 아니라 **DiscountPolicy 인터페이스 하나만의 의존**하는 형태로 변경하였습니다.

이를 통해 **새로운 전략이 추가되는 경우**, 콘텍스트(Pos)의 코드를 변경할 필요 없이 DiscountPolicy의 **콘크리트 클래스를 생성**하는 것으로 새로운 전략을 추가할 수 있게 되었습니다.

그리고 한 가지 짚고 넘어가야 하는 부분이 있습니다.

앞서 전략 패턴을 이야기 할때, **"콘텍스트의 클라이언트가 콘텍스트에 사용할 전략을 전달해 줍니다."** 라고 이야기 했습니다. 첫 방문 회원에게 할인을 적용하는 시나리오를 예로 들면,

<a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-client-scenario.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-client-scenario.png" title="Check out the image">
</a>

Pos의 클라이언트는 첫 방문 회원 할인이 필요한 경우, 첫 방문 회원 할인 버튼을 호출해서 전략(첫 방문 회원 할인)에 해당하는 콘크리트 클래스를 생성합니다.

이 처럼 콘텍스트의 클라이언트는 **구체적인 전략인 콘크리트 클래스와 쌍을 이루고** 콘텍스트에 사용할 전략을 전달해 줍니다.

## 결론
* 전략 패턴의 이점은 **콘텍스트 코드의 변경 없이 새로운 전략을 추가** 할 수 있습니다.
* 전략 패턴을 적용함으로써 새로운 정책 확장에는 열려 있고 변경에는 닫혀 있게됩니다. 즉, **개방 폐쇄 원칙**을 따르는 구조를 갖게 됩니다.


추가로 **"마감 직전 손님에게 20% 할인"** 한다는 새로운 요구사항 추가된다고 할때, 아래와 같이 새로운 전략이 추가되어도 **콘텍스트 코드의 변경없이 새로운 전략을 추가할 수 있음**을 볼 수 있습니다.

<a href="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-new-strategy-scenario.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.designpattern_img }}/designpattern-strategy-new-strategy-scenario.png" title="Check out the image">
</a>


## References
* 개발자가 반드시 정복해야할 객체지향과 디자인 패턴 - 최범균