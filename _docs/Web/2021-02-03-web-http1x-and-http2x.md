---
title: HTTP1.1와 HTTP/2
category: Web
date:   2021-02-03 00:30:59
comments: true
order: 10
---


## 목차
* HTTP 개요
* HTTP/1.1
* HTTP의 확장
  + 보안 전송을 위한 HTTP
  + 복잡한 애플리케이션을 위한 HTTP
  + 웹의 보안 모델 완화
* HTTP 1.1 vs HTTP/2
* 차세대 HTTP
* References

## HTTP 개요
**HTTP(HyperText Transfer Protocol)**는 WWW(World Wide Web)에 내재된 프로토콜로 팀 버너스리(Tim Berners-Lee)에 의해 1989~1991년에 발명되었습니다. HTML과 같은 하이퍼미디어 문서를 전송하기 위한 **Application Layer의 프로토콜**입니다.

HTTP 초기 버전에는 버전 번호가 없었습니다. 그러나 차후 버전과 구별하기 위해 버전 번호가 붙었고 다음과 같이 발전해왔습니다.

* HTTP/0.9
* HTTP/1.0
* HTTP/1.1
* HTTP/2

이번 게시글에서는 **HTTP/1.1**와 **HTTP/2** 에 대해서 알아보겠습니다.

> HTTP와 자세한 내용은 이곳 [HTTP]()를 참고해주세요.

## HTTP/1.1
**HTTP의 첫번째 표준 버전인 HTTP/1.1**은 HTTP/1.0이 나온지 몇 달 안되서 1997년 초에 공개되었습니다.

HTTP/1.1은 모호함을 명확하게 하고 많은 개선 사항들을 도입했습니다.

* **커넥션의 재사용**: 탐색된 단일 원본 문서에 임베드된 리소스들을 보여주기 위해 사용된 커넥션을 다시 열어 시간을 절약
* **파이프라이닝 추가**: 첫번째 요청에 대한 응답이 완전히 전송되기 이전에 두번째 요청 전송을 가능하게 해서 통신 지연율을 낮춤
* **청크된 응답 지원**
* **캐시 제어 메커니즘 도입**
* **언어, 인코딩 타입을 포함한 컨텐츠 협상 도입**: 클라이언트와 서버로 하여금 교환하려는 가장 적합한 컨텐츠에 대한 동의 가능
* **Host 헤더**: 동일 IP주소에 다른 도메인을 호스트 가능

## HTTP의 확장
#### 보안 전송을 위한 HTTP
HTTP는 기본적으로 **TCP/IP 스택**을 기반으로 연결합니다. 그리고 넷스케이프 커뮤니케이션즈([Netscape Communications](https://ko.wikipedia.org/wiki/%EB%84%B7%EC%8A%A4%EC%BC%80%EC%9D%B4%ED%94%84))는 이것의 토대 위에 추가적인 **암호화된 전송 계층인 SSL**을 만들었습니다. 그리고 이후 **SSL이 표준화 되어 TLS**가 되었습니다.

#### 복잡한 애플리케이션을 위한 HTTP
기존 웹은 단순히 읽기 전용의 매체는 아니였습니다. 팀버너스리는 사람들이 문서를 원격으로 추가, 이동 가능한 분산된 파일 시스템의 한 종류로 웹을 상상했었습니다. 1996년도 쯤 HTTP는 저작을 허용하도록 확장되었고 WebDAV라는 표준이 만들어 지면서 특정 애플리케이션들로 확장 되어왔습니다.

그리고 2000년에, HTTP 사용에 대한 새로운 패턴이 고안되었는데 그것이 **REST(REpresentational State Transfer)** 입니다. 이것은 웹 애플리케이션으로 하여금 브라우저나 서버의 갱신없이 데이터 탐색과 수정을 허용하는 API 제공이 가능해졌습니다.

#### 웹의 보안 모델 완화
웹 보안 모델은 HTTP가 만들어진 이후에 개발되어 왔습니다. 이러한 내용들은 [Cross-Origin Resource Sharing(CORS)](https://developer.mozilla.org/en-US/docs/Glossary/CORS) 혹은 [컨텐츠 보안 정책(CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) 과 같은 스펙 내에 정의되어있습니다.

## HTTP/1.1와 HTTP/2
HTTP/1.1의 커넥션은 올바른 순서로 전송되는 요청을 필요로 합니다. 때문에 몇 이론적으로 병렬 커넥션이 가능한 몇몇 경우에는 여전히 많은 양의 오버헤드와 복잡도가 남아있습니다.

이러한 점에서 **HTTP/2 프로토콜은 HTTP/1.1 버전과 다른 몇가지 차이점**을 가지고 있습니다.

* **텍스트 프로토콜이 아닌 이진 프로토콜이다.** 더 이상 읽을 수도 없고 수작업을 만들어낼 수 없기때문에 새로운 최적화 기술이 구현될 수 있습니다.
* **병렬 요청이 가능한 다중화 프로토콜이다.** 동일한 커넥션 상에서 병렬 요청이 가능합니다. 이러한 점은 HTTP/1.x 프로토콜의 제약사항을 막아줍니다.
* **데이터 중복으로 인한 오버헤드 제거**
* **연속된 요청에서의 유사한 내용의 헤더들 압축**
* 서버로 하여금 사전에 클라이언트 캐시를 서버 푸쉬라고 불리는 매커니즘에 의해, 필요하게 될 데이터로 채워넣기는 것을 허용

## 차세대 HTTP
HTTP는 HTTP/2의 릴리즈와 함께 계속된 진화를 거듭하고 있습니다. HTTP의 확장성은 여전히 새로운 기능들을 추가하는데 사용되고 있습니다. 다음은 2016년에 나타난 HTTP의 새로운 확장들을 볼 수 있습니다.

* **Alt-Svc** 신분 증명의 개념과 주어진 자원의 위치를 분리하도록 해줍니다.
* **Client-Hints** 클라이언트(브라우저)의 요구사항이나 서버의 하드웨어 제약사항에 관한 정보를 사전에 미리 주고 받을 수 있습니다.
* **Cookie** Cookie 내에 보안 관련 접두사 도입은 보안 쿠키가 변경되지 않았다는 것을 보장하는데 도움을 줍니다.

## References
* [HTTP - MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/HTTP)
* [Evolution of HTTP - MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)
* [HTTP/1.1 - rfc2068](https://tools.ietf.org/html/rfc2068)