---
title: MSA(MicroService Architecture)
category: Tech
date: 2020-09-28 00:30:59
lastmod: 2020-09-30 00:30:59
comments: true
order: 2
---


## 목차
* MSA 등장 배경
  + Monolithic Architecture의 이해
* MSA 란?
* MSA 특징
* MSA 장점, 단점


## MSA 등장 배경
MSA는 웹기반의 분산 시스템의 디자인에 많이 반영되고 있는 아키텍쳐 입니다. 특정 사람이 정의한 아키텍쳐가 아니라, 분산 웹 시스템의 구조가 유사한 구조로 설계 되면서 개념적으로만 존재하였습니다. 

마틴파울러(Martin Fowler)가 2014년도에 MSA에 대한 개념을 글로 [정리](https://martinfowler.com/articles/microservices.html)하여 개념을 정립 시키는데 일조를 하였습니다.

MSA를 이해 하기 이전에 __Monolithic Architecture__ 스타일에 대해서 이해해야 합니다.

#### Monolithic Architecture
![tech-monoliths-and-msa]({{ site.baseurl }}{{ site.tech_img }}/tech-monoliths-and-msa.JPG)

__Monolithic Architecture__ 란 소프트웨어의 모든 구성요소가 한 프로젝트에 통합되어있는 형태입니다. 

기존의 전통적인 웹 시스템 개발 스타일로, __하나의 애플리케이션 내에 모든 로직들이 들어가 있는 구조__ 입니다.

예를 들어, 온라인 쇼핑몰 애플리케이션이 있을때, 톰캣 서버에서 동작하고 있는 WAR 파일 내에, 사용자 관리, 상품, 주문 등 모든 컴포넌트들이 들어있고 이를 처리하는 UX 로직까지 하나로 포장되서 들어가 있는 구조입니다. 

##### 특징
* 하나의 애플리케이션 안에 __모든 컴포넌트들이 들어있습니다.__
* 각 컴포넌트들은 상호 호출을 함수를 이용한 __call-by-reference 구조__ 를 가집니다.
* 소규모 프로젝트에서 용이한 구조입니다.

##### 장점
전체 애플리케이션을 하나로 처리하기 때문에 다음과 같은 장점이 있습니다.

* call-by-reference에 의해 컴포넌트간 호출시 __성능에 제약이 덜합니다.__
* 하나의 애플리케이션으로 되어있어 __트랜잭션 관리에 용이합니다.__
* 하나의 애플리케이션만 운영하기 때문에 __배포가 간편__ 합니다. 
* 하나의 애플리케이션만 __테스트 수행__ 하면 되기 때문에 편리합니다.

##### 단점 
Monolithic 구조는 일정 규모 이상의 서비스, 많은 개발 인력이 투입되는 프로젝트에서는 해당 스타일은 한계를 보입니다.

* 프로젝트 진행 관점에서 프로젝트가 커질 수록, 많은 개발 인력이 투입될 경우 __협업 개발이 쉽지 않습니다.__
* 프로젝트가 커질 수록 __전체 시스템의 구조와 특성을 이해하는 것이 어려워 집니다.__
  + 컴포넌트들이 서로 call-by-reference 기반으로 연결되어 있기 때문에 전체 시스템의 구조를 제대로 파악하지 않고 개발을 진행하면, 특정 컴포넌트나 모듈에서의 성능 문제나 장애가 다른 컴포넌트에까지 영향을 주게 되며, 이런 문제를 예방하기 위해서는 개발자가 대략적인 전체 시스템의 구조 등을 이해 해야 하기 때문입니다.
* __잦은 배포__ 가 있는 시스템의 경우 불리한 구조입니다.
  + 특정 컴포넌트를 수정하고자 했을때, 컴포넌트 재 배포시 수정된 컴포넌트만 재 배포 하는 것이 아니라 전체 애플리케이션을 재 컴파일 하여 전체를 다시 통으로 재배포 해야하기 때문입니다.

> ※참고※<br>
> MSA 이전에 CBD, SOA 등 Monolith를 논리, 물리적으로 구조화 하기 위한 노력들이 있었습니다.<br> 
> 또한, MSA는 SOA의 부분집합으로 여겨지고 있습니다.

## MSA(MicroService Architecture) 란?
__MSA(MicroService Architecture) 는 "하나의 큰 어플리케이션을 여러개의 작은 어플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍쳐" 입니다.__

MSA는 __대용량 웹서비스__ 가 많아짐에 따라 정의된 아키텍쳐입니다. __MSA의 근간은 SOA(Sevice Oriented Architecture: 서비스 지향 아키텍쳐)__ 에 두고있습니다.

> SOA는 Enterprise 시스템을 중심으로 고안된 아키텍쳐라면, MSA는 SOA 사상에 근간을 두고 대용량 웹서비스 개발에 맞는 구조로 사상이 경량화 되고, 대규모 개발팀의 조직 구조에 맞도록 변형된 아키텍쳐입니다.

#### MAS 구조
* 기본 구조, 배포 관점 구조
* 데이터 저장 관점

![tech-msa-structures]({{ site.baseurl }}{{ site.tech_img }}/tech-msa-structures.JPG)

마이크로 서비스 아키텍쳐의 __기본 구조__ 는 각 컴포넌트가 서비스라는 형태로 구현되고 API를 이용하여 타 서비스와 통신을 합니다.

__배포 구조__ 관점 에서 볼때, 각 서비스는 독립된 서버로 타 컴포넌트와의 의존성이 없이 독립적으로 배포 됩니다.

예를 들어 사용자 관리 서비스는 독립적인 war파일로 개발되어, 독립된 톰캣 인스턴스에 배치됩니다. 확장을 위해서 서비스가 배치된 톰캣 인스턴스는 횡적으로 스케일(톰캣 인스턴스 수를 추가)이 가능하고, 앞단에 로드 밸런서를 배치하여 서비스간의 로드를 분산 시킵니다.

![tech-monolith-and-msa-database]({{ site.baseurl }}{{ site.tech_img }}/tech-monolith-and-msa-database.JPG)

__데이터 저장관점__ 에서는 중앙 집중화된 하나의 데이터 베이스를 사용하는 것이 아니라 서비스 별로 별도의 데이터 베이스를 사용합니다. Monolithic 구조의 경우 하나의 데이터 베이스(RDBMS)를 사용하는 경우가 일반적입니다. 마이크로 서비스 아키텍쳐의 경우, 서비스가 API에서 부터 데이터 베이스까지 분리되는 수직 분할 원칙(Vertical Slicing)에 따라서 독립된 데이터 베이스를 가집니다. 또한 이 경우, 다음과 같은 __장단점__ 을 가지고 있습니다.

* __장점__
  + 다른 서비스 컴포넌트에 대한 의존성이 없이 서비스를 독립적으로 개발 및 배포/운영할 수 있다.
* __단점__
  + 다른 컴포넌트의 데이타를 API 통신을 통해서만 가지고 와야 하기 때문에 성능상 문제를 야기할 수 있다.
  + 데이터 베이스간의 트랜잭션을 묶을 수 없는 문제점이 있다.(이후 추가 설명)


## MSA 특징
* API Gateway
* Orchestration
* 공통 기능 처리 (Cross cutting function handling)


## References
* [bcho.tistory.com/948](https://bcho.tistory.com/948)
* [velog.io/@tedigom](https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e)
* [martinfowler/microservices](https://martinfowler.com/articles/microservices.html)
* [www.samsungsds.com](https://www.samsungsds.com/global/ko/support/insights/msa.html)
