---
title: CI/CD(지속적 통합/지속적 배포)
category: Tech
date: 2020-09-30 00:30:59
comments: true
order: 3
---

## 목차
* CI/CD 배경
* CI/CD란?
* CI(Continus Integration)
  + 장점
  + CI도구
* 지속적 제공
* CD(Continus Deployment)
  + CD도구 

## CI/CD 배경
현대의 웹 서비스 개발에서는 __하나의 프로젝트를 여러 개발자가 협업을 통해 개발__ 을 진행합니다. 그러다 보니 각자가 개발한 코드를 합쳐야 할때마다 어려움이 있었습니다. 그래서 매주 코드 병합일(Merge)을 정해 병합작업을 하였습니다. 이러한 수작업은 개발 생산성에 좋지 않아 __지속해서 코드가 통합되는 환경(CI)을 구축__ 하게 되었습니다.

CI환경이 구축되어 개발자들이 작성한 코드를 저장소(git, github etc)에 PUSH가 될 때마다 코드 병합, 테스트 코드와 빌드를 수행하면서 자동으로 코드가 통합되어 개발자들은 개발에만 집중할 수 있게 되었습니다.

1~2대의 서버에는 수동으로 배포가 가능하지만, 10~100대의 서버에 배포를 해야하거나 긴급하게 배포를 해야하는 상황에서 수동으로 배포할 수 없습니다. 그래서 수동이 아닌 __자동으로 배포 가능한 환경(CD)__ 를 구축하게 되었습니다. 


## CI/CD란?
* CI/CD는 다음과 같은 의미를 가지고 있습니다. __지속적 통합__(CI, Continus Integration)과 __지속적 배포__(CD, Continus Deployment)

* CI/CD는 __애플리케이션 개발 단계를 자동화__ 하여 애플리케이션을 보다 짧은 주기로 고객에게 제공하는 방법입니다.

* CI/CD는 __새로운 코드 통합 으로 인해__ 개발 및 운영에서 __발생하는 문제__ 를 해결하기 위한 솔루션입니다.

* CI/CD는 애플리케이션 통합 및 테스트 단계에서 제공 및 배포에 이르는 애플리케이션의 라이프사이클 전체에 걸쳐 __지속적인 자동화와 지속적인 모니터링을 제공__ 합니다. 이러한 일련의 구축 과정을 __"CI/CD 파이프라인"__ 이라고 부릅니다.

CI/CD의 __전체적인 흐름__ 은 다음과 같습니다.

<a href="{{ site.baseurl }}{{ site.tech_img }}/tech-ci-cd-flow.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.tech_img }}/tech-ci-cd-flow.JPG" title="Check out the image">
</a>

## CI(Continus Integration)
__CI(Continus Integration - 지속적인 통합)__ 는 코드 버전 관리 시스템 VCS(ex. svn, git etc)에 PUSH가 되면 자동으로 테스트와 빌드가 수행되어 안정적인 배포 파일을 만드는 과정을 말합니다.

CI는 개발자를 위한 __자동화 프로세스__ 입니다. 

> CI 도구를 도입 혹은 환경을 구축하고, 코드 변경 사항을 병합할때 클래스와 기능에서부터 전체 애플리케이션을 구성하는 서로 다른 모듈에 이르기까지 모든 것에 대한 __테스트 코드__ 를 작성하고 이를 통해 애플리케이션의 변경 사항이 제대로 적용되었는지를 확인해야합니다.

#### 장점
CI를 구현한 경우, 애플리케이션에 대한 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 공유 리포지토리(ex. github)에 통합되므로 여러 명의 개발자가 동시에 애플리케이션 개발과 관련된 코드 작업을 할 경우 __서로 충동할 수 있는 문제를 해결__ 할 수 있습니다.

한 사람의 작업이 다른 사람의 작업에 어떤 일이 일어났는지 깨닫지 못한 채 __다른 사람의 작업을 밟은 버그를 찾는 데 시간을 소비하는 문제를 해결__ 할 수 있습니다.

#### CI 도구
* Travis CI
  + 무료(private repo는 유료)
  + 깃허브 제공 CI 서비스
  + 미설치형(추가 서버 필요 없음)
* 젠킨스(Jenkins)
  + 무료
  + 설치형(추가 서버 필요)
* AWS(Code Build)
  + 유료(빌드 시간만큼 비용 부과)

## 지속적 제공(Continus Delivery)
__지속적 제공(Continus Delivery)__ 는 CI의 빌드 자동화, 유닛 및 통합 테스트 수행 후 이어지는 프로세스입니다.

지속적 제공(Continus Delivery)은 코드 변경 사항 병합, 테스트 자동화, 코드 릴리스 자동화가 포함됩니다.

## CD(Continus Deployment)
__CD(Continus Deployment - 지속적인 배포)__ 는 CI를 통한 빌드 결과를 자동으로 운영 서버에 무중단 배포까지 진행되는 과정을 말합니다.

#### CD 도구
* AWS(Code Deploy)

## Reference
* [martinfowler/articles/ci](https://martinfowler.com/articles/continuousIntegration.html)
* [martinfowler/articles/originalci](https://www.martinfowler.com/articles/originalContinuousIntegration.html)
* [redhat.com](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)