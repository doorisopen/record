---
title: abstract와 interface
category: Java
date:   2020-06-22 00:30:59
comments: true
order: 2
---

## abstract 과 interface 의 차이는 무엇인가
#### abstract
__추상 메소드(abstract method)란__ 자식 클래스에서 반드시 오버라이딩해야만 사용할 수 있는 메소드를 의미합니다. 자바에서 추상 메소드를 선언하여 사용하는 목적은 추상 메소드가 포함된 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함입니다.

예를 들면 모듈처럼 중복되는 부분이나 공통적인 부분은 미리 다 만들어진 것을 사용하고, 이를 받아 사용하는 쪽에서는 자신에게 필요한 부분만을 재정의하여 사용함으로써 생산성이 향상되고 배포 등이 쉬워지기 때문입니다.

#### interface
__인터페이스(interface)란__ 자식 클래스가 여러 부모 클래스를 상속받을 수 있다면, 다양한 동작을 수행할 수 있다는 장점을 가지게 될 것입니다. 하지만 클래스를 이용하여 다중 상속을 할 경우 메소드 출처의 모호성 등 여러 가지 문제가 발생할 수 있어 자바에서는 클래스를 통한 다중 상속은 지원하지 않습니다.

하지만 다중 상속의 이점을 버릴 수는 없기에 자바에서는 인터페이스라는 것을 통해 다중 상속을 지원하고 있습니다. 인터페이스(interface)란 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스를 의미합니다.

자바에서 추상 클래스는 추상 메소드뿐만 아니라 생성자, 필드, 일반 메소드도 포함할 수 있습니다. 하지만 인터페이스(interface)는 오로지 추상 메소드와 상수만을 포함할 수 있습니다.

## 결론
인터페이스는 구현해내는 클래스에서 인터페이스의 모든 method 들을 반드시 override 해야만한다. 반면 abstract는 abstract로 선언된 method 만 구현하면된다.