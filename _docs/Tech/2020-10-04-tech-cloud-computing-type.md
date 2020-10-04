---
title: 클라우드 컴퓨팅 유형(public, private)
category: Tech
date: 2020-10-04 00:30:59
comments: true
order: 5
---



## 클라우드 컴퓨팅
클라우드 컴퓨팅에는 4가지 유형이 있습니다.

* 퍼블릭(public) 클라우드
* 프라이빗(private) 클라우드
* 하이브리드(hybrid) 클라우드
* 멀티(multi) 클라우드

위와 같은 유형들의 차이점은 넓게 보면 __위치와 소유권__ 이라고 할 수 있습니다.

## 퍼블릭(public) 클라우드
__퍼블릭(public) 클라우드__ 는 누구나 함께 이용할 수 있게 구축된 대규모 클라우드 서비스입니다. 사용자는 필요할 때에 필요한 만큼의 클라우드 자원을 할당받아 이용할 수 있도록 제공하는 서비스 방식입니다. 필요한 서버를 즉시 생성하여 이용할 수 있으며, 트래픽 및 자원 사용량 증가시 바로 확장이 가능한 확장성, 저렴한 서비스 비용으로 경제성, 시스템 유지보수나 장애 대응을 위한 효율성 등 클라우드 서비스의 장점에 최적화된 서비스 입니다.

퍼블릭(public) 클라우드에는 다음과 같은 서비스들이 있습니다.

* AWS
* Google Cloud
* IBM Cloud
* Microsoft Azure

위와 같은 퍼블릭(public) 클라우드 공급업체가 베어 메탈(bare metal) IT 인프라도 추상화하여 IaaS 형태로 판매하거나 클라우드 플랫폼으로 개발하여 PaaS 형태로 판매할 수 있습니다.

> __베어 메탈(bare metal) 이란?__ 어떠한 소프트웨어도 담겨 있지 않은 하드웨어를 가리킵니다. 다른 관점에서 보면 하드웨어를 구매할 수 있는 상품을 의미합니다.<br/>
> __베어 메탈 서버(Bare Metal Server)__ 는 클라우드 환경에서 편리하게 고성능의 물리 서버를 관리할 수 있습니다.

## 프라이빗(private) 클라우드
__프라이빗(private) 클라우드__ 는 자사의 내부에 직접 클라우드 인프라를 구축한 형태로 클라우드 인프라에 대한 직접적인 통제 권한을 가진 클라우드 서비스 구축 방식을 말합니다. 

일반적으로 사설 클라우드를 구축하는 경우에는 보안을 위해 외부의 인터넷망과의 IT 인프라를 분리하거나, 운영 회사에서 원하는 형태로 보안 시스템과 내부 IT 인프라를 구성할 수 있습니다. 그러나 퍼블릭(public) 클라우드와 달리 구성된 여유 자원이 한정되어 있으므로 예기치 않은 사건(자원의 필요량 증가)에 따른 확장에 제약이 있을 수 있으며, 직접 구축으로 인한 비용의 증가와 클라우드 인프라를 관리하기 위한 관리 인력이 필요합니다.

많은 기업과 공공기관들이 보안에 대한 민감도가 높은 IT 시스템을 운영하기 위해 프라이빗(private) 클라우드를 구축 및 운영하고 있습니다. 주로 __외부의 접속이 필요없이 회사 내부 서비스__ 를 위한 그룹웨어, ERP와 같은 서비스들이 주로 사설 클라우드 환경으로 구성됩니다.

## Reference
* [blog.naver/didim365](https://blog.naver.com/didim365_/220603372846)
* [redhat/cloud-computing-type](https://www.redhat.com/ko/topics/cloud-computing/public-cloud-vs-private-cloud-and-hybrid-cloud)
* [blog.naver/n_cloudplatform](https://m.blog.naver.com/PostView.nhn?blogId=n_cloudplatform&logNo=221212685823&proxyReferer=https:%2F%2Fwww.google.com%2F)