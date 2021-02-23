---
title: Session vs Token vs Cookie
category: Web
date:   2021-02-23 00:30:59
comments: true
order: 8
---


## 목차
* 이것이 왜 필요한가?
* Session
* Cookie
* Session과 Cookie 차이점
  + Cookie를 사용하는 이유?
* Session과 Cookie 한계점
  + 서버 확장시 세션 정보의 동기화 문제
  + 서버, 세션 저장소의 부하
  + CORS(Cross-Origin Resource Sharing)
* Token
  + 동작순서
  + 특징(단점)
  + 토큰 저장방식(웹 스토리지, 쿠키)
  + 토큰 사용 예시
* Token과 Cookie
* 결론
* References

## 이것이 왜 필요한가?

우선 이들이 생겨난 이유(?)에 대해서 생각해보면 HTTP의 특성 때문이라고 할 수 있습니다. HTTP의 가장 큰 특징은 **무상태(Statusless), 비연결성(Connectionless)**라는 점입니다.

**무상태(Statusless)란** "**서버가 클라이언트의 상태를 보존하지 않는다."**라는 의미입니다. 다시말해 첫 번째 통신에서 데이터를 주고 받았다고 해도 두 번째 통신에서 이전 데이터를 유지하지 않습니다.

* 장점: 클라이언트와 서버의 연결고리가 없기 때문에 서버 확장성(스케일 아웃)이 좋다.
* 단점: 클라이언트가 데이터를 추가로 전송해야한다.

**비연결성(Connectionless)은 "클라이언트와 서버가 한 번 연결을 맺은 후, 요청에 대한 응답을 마치면 해당 연결을 끊어 버린다."** 라는 의미입니다. 그리고 인터넷은 불특정 다수의 통신 환경이라는 점에서 다음과 같은 장단점을 갖습니다.

* 장점: 빠른 응답 속도, 서버 자원을 매우 효율적으로 사용 가능하다.
* 단점: 서버는 클라이언트 정보를 기억하고 있지 않아 매번 연결/해제에 대한 오버헤드가 발생한다.(HTTP1.1의 지속연결(Keep Alive 속성)으로 해당 문제 해결 가능합니다.)

위의 특성을 예를 들어 설명하면, 온라인 쇼핑몰을 이용하는 사용자가 원하는 상품을 선택하고 구매하기 위해 구매 버튼을 눌렀다고 가정하면 **해당 쇼핑몰은 클라이언트의 정보를 보존하지 않기 때문에** 사용자가 선택한 상품의 정보 또한 유지 하지 못하는 상황을 경험하게 될것입니다. 이처럼 클라이언트의 정보를 유지가 필요한 상황 경우가 많습니다.

따라서, **클라이언트의 상태를 유지(Stateful)**가 필요한 경우에 대처하기 위해 **쿠키, 세션 그리고 토큰**을 사용합니다.

---

# Session

## 정의(wiki)

**세션(Session)**은 **네트워크 분야**에서 반영구적이고 **상호작용적인 정보 교환을 전제**하는 둘 이상의 통신 장치나 컴퓨터와 사용자 간의 대화나 송수신 연결상태를 의미하는 **보안적인 다이얼로그(dialogue)* 및 시간대**를 가리킨다. 쉽게 말해 **일정 시간 동안 사용자로 부터 들어오는 일련의 요청을 하나의 상태로 보고 그것을 일정하게 유지하기 위한 기술**입니다.

**쉽게 말해, 사용자가 웹 서버에 접속해 있는 상태를 하나의 세션이라고 합니다.**

*다이얼로그(dialogue): 연극이나 영화에서, 작중 인물들 사이에 이루어지는 대화

## 특징

* 상태를 유지하기 위한 정보를 서버가 관리하고 저장합니다.
* 각 클라이언트에게 고유 sessionId를 부여합니다.
* sessionId는 브라우저 종료시 소멸됩니다.(세션은 브라우저 단위)
* 저장 데이터에 제한이 없습니다.(서버 용량 허용 이내로)
* 세션 timeout 기본 설정 시간은 30분(3600초)입니다.(변경 가능)

## 동작 순서
<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-session.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-session.png" title="Check out the image">
</a>

1. 클라이언트가 서버에 로그인 Request를 보냅니다.
2. 서버는 클라이언트의 Request-Header 필드인 Cookie를 확인하여 sessionId 유무를 확인합니다.
3. 존재하지 않으면 sessionId를 새로 발급하여 반환합니다.(cookie name: JSESSIONID)
    1. 존재하면 로그인한 사용자 정보로 갱신하여 새로운 sessionId를 발급하여 반환합니다.(로그아웃 하면 새로운 사용자로 인식해서 새로운 세션을 생성)
    2. 서버는 sessionId를 서버에 저장합니다.
4. 클라이언트는 sessionId 값을 매 요청마다 헤더 쿠키에 넣어서 요청합니다.
5. 서버는 전달 받은 쿠키에서 sessionId를 통해 사용자를 식별합니다.
6. 클라이언트(브라우저) 종료 시 sessionId를 제거합니다.(서버에서도 세션 제거)

## 저장 방식
* WAS의 In Memory에 저장
  + 배포할 때마다 애플리케이션 재실행되고 Memory 초기화됨
  + WAS가 2대 이상 구동하는 환경인 경우에 세션 동기화가 필요함
  + 서버의 제한적인 메모리 용량으로 과부화 문제
* DB(MySQL 등)에 저장
  + 2대 이상 구동하는 환경에서 공용 세션으로 사용할 수 있는 대안책
  + 사용자가 많아짐에 따라 DB IO가 많아짐 → 성능 이슈 발생
* Memory DB(Redis, Memcache 등)에 저장

# Cookie
## 정의(wiki)
**쿠키(Cookie)란** HTTP의 일종으로서 인터넷 사용자가 어떠한 웹사이트를 방문할 경우 그 사이트가 사용하고 있는 서버를 통해 인터넷 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일을 일컫습니다.

**쿠키(Cookie)**는 서버를 대신해서 클라이언트의 정보들을 사용자 로컬PC에 저장하고 서버에 요청을 보낼 때 쿠키도 함께 보내서 서버가 사용자를 식별할 수 있게 해줍니다.

## 사용 목적
* **세션 관리(Session Management)** 로그인, 사용자 닉네임, 접속 시간, 장바구니 등 클라이언트 정보를 저장합니다.
* **개인화(Personalization)** 사용자마다 다르게 개인 페이지를 보여줄 수 있습니다.
* **트래킹(Tracking)** 사용자 행동과 패턴을 분석하고 기록하여 광고를 노출시킬 수 있습니다.

## 사용 예시
* ID 저장, 로그인 상태 유지
* 팝업창 24시간 동안 보지 않기
* 최근 검색한 내역
* 장바구니

## 특징
* key, value 형태의 데이터 구조를 가지고 String 타입입니다.
* 브라우저마다 저장되는 쿠키는 다릅니다.(Chrome에서 생성된 쿠키는 IE에서 사용 불가)
  + 서버에서 브라우저가 다르면 다른 사용자로 인식합니다.
* 클라이언트에 총 300개의 쿠키를 저장할 수 있습니다.(쿠키 1개: 4KB(=4096byte))
* 하나의 도메인 당 20개의 쿠키를 가질 수 있습니다.

## 동작 순서
<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-cookie-flow.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-cookie-flow.png" title="Check out the image">
</a>

1. 클라이언트가 서버에 로그인 요청을 보냅니다.
2. 서버는 세션을 생성합니다.
3. 서버는 요청한 페이지를 반환하는 동시에 정보를 담은 쿠키를 반환합니다.
4. 반환받은 쿠키는 로컬PC에 저장하고 있다가 서버에 Request를 보낼 때 쿠키를 함께 전송합니다.
5. 동일 사이트 재방문시 클라이언트의 PC에 쿠키가 있는 경우, 요청 페이지와 함께 전송합니다.

## Session과 Cookie 차이점

||세션(Session)|쿠키(Cookie)|
|:---:|:---:|:---:|
|저장 위치|서버에 저장|사용자PC(사용자 컴퓨터)에 저장|
|저장 형식|text로 저장|Object로 저장|
|만료 시점|브라우저 종료 시 삭제|쿠키 저장 시 설정|
|사용 자원|웹 서버 자원 사용|클라이언트 자원|
|용량|제한 없음(서버 허용 이내)|총 300개(도메인당 20개)|
|속도|느림|빠름|
|보안|좋음|나쁨|

#### Cookie를 사용하는 이유?
세션의 경우 **서버에 저장**되고, 서버 자원을 사용하기 때문에 사용자가 많을 경우 소모되는 리소스가 많습니다. 자원관리 차원에서 세션, 쿠키를 적절히 병행 사용하면 리소스 낭비를 방지하고 웹사이트의 속도를 높일 수 있습니다.

#### 1.서버 확장시 세션 정보의 동기화 문제

<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-1.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-1.png" title="Check out the image">
</a>

위에서 Session 저장 방식에서 언급했던 방식인 **WAS In Memory에 세션 저장**을하게 될 때의 문제점입니다. 일반적으로 세션에 대한 별다른 설정을 하지 않았다면 세션은 In Memory에 저장이 될 것입니다. 그러나 사용자가 늘어남에 따라 서버 또한 확장할 필요가 있는데 이때 **각 서버에 세션 정보를 동기화** 해주어야 합니다.

그렇지 않으면 로그인후(서버1) 페이지를 이동하다보면(서버2로 연결) 인증이 풀려있는 상황을 경험하게 될 수도 있습니다.

#### 2.서버, 세션 저장소의 부하

<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-2.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-2.png" title="Check out the image">
</a>

세션 정보를 In Memory에 저장하지 않고 MySQL과 같은 데이터베이스를 세션 저장소로 사용했을때 발생하는 문제점입니다.

이 방법은 In Memory의 문제점(세션 동기화 문제)을 해결할 수 있는 가장 간단한 방법입니다. 그러나, 사용자가 많아짐에 따라 DB 부하를 야기할 문제점이 있습니다.

#### 3.CORS(Cross-Origin Resource Sharing)

<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-3.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-problem-3.png" title="Check out the image">
</a>

쿠키는 단일 도메인 및 서브 도메인에서만 작동하도록 설계되어 여러 도메인에서 관리하기 번거롭습니다.

그리고, **Web-App간의 상이한 쿠키-세션 처리 로직**을 가지고 있습니다. 따라서 모바일로 접근하는 경우에 적절한 처리가 필요합니다.(모바일의 경우 쿠키 컨테이너를 사용해야 합니다.) 

---

앞서 이야기한 내용을 다시금 정리하면 다음과 같습니다.

<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-cookie-flow.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-cookie-flow.png" title="Check out the image">
</a>

Session 정보는 **서버**에 그리고 클라이언트에 관한 정보(좋아하는 글자색, 즐겨찾기 등)는 **Cookie에 저장** 및 관리합니다. 

근데 서버는 **더 많은 정보들을 쿠키에 저장하지 않는 이유**는 무엇일까?

클라이언트로 부터 오는 정보들을 **신뢰할 수 없기 때문**입니다. 이에 대한 대안은 클라이언트에 위와 같은 **정보들을 저장하고 서명하는 것**입니다. 서명을 보유한 사람은 누구나 데이터가 조작되었는지 여부를 신속하게 확인할 수 있습니다.

**서버 기반 인증 방식(Session, Cookie)**은 아직도 많이 사용 되고 있습니다. 하지만, 요즘 **웹과 모바일 웹 어플리케이션들이 많아지면서 앞서 본 다음과 같은 문제점들이 보이기 시작**했습니다.

- 세션 동기화
- DB 과부화
- CORS(Cross-Origin Resource Sharing)

그리고 이러한 문제점들을 해결하는 **최선의 방법은 Token 기반 인증을 도입**하는 것 입니다.

# Token
토큰 기반의 인증 방식은 보호할 데이터를 토큰으로 치환하여 사용하는 기술입니다.

## 동작 순서
토큰 기반 시스템의 구현 방식은 시스템마다 차이가 있습니다.

<a href="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-token-flow.png" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.web_img }}/web-session-token-cookie-token-flow.png" title="Check out the image">
</a>

1. Client가 로그인 요청을 합니다.
2. 서버 측에서 **계정 정보를 검증**합니다.
3. 검증을 성공하면 **Signed token을 발급**해줍니다.(signature를 가진 토큰)
4. 클라이언트는 **토큰을 저장**하고, 서버에 요청을 할 때 마다, 해당 **토큰을 함께 서버에 전달**합니다.
5. 서버는 **토큰을 검증하고 요청에 응답**합니다.

## 특징
* **토큰 기반 시스템**은 **stateless(무상태)** 입니다. 즉, 클라이언트의 상태를 유지하지 않는다는 것입니다.
  + 토큰은 클라이언트 사이드에 저장하기 때문에 완전히 stateless 합니다.
* **토큰 기반 시스템**은 **사용자의 인증 정보를 서버 혹은 세션에 담아두지 않습니다**.
  + 이로 인해 유저의 인증 정보를 서버측에 담아둠으로서 발생하는 취약점이 사라졌습니다.(토큰도 취약점이 존재할 수 도 있습니다.)
* 토큰 기반 시스템은 로그인 정보 사용 등의 **확장성(Extensibility)**이 좋습니다. ****토큰을 사용하여 다른 서비스에서 권한 공유가 가능합니다.(OAuth)
  + ex) 만약 세션을 서버측에 저장하고, 여러대 서버를 운영하는 환경에서, 어떤 유저가 로그인 했을때, 그 유저는 처음 로그인했던 서버에만 요청을 보내도록 설정을 해야합니다. 그러나 토큰(signed token)을 사용한다면 어떤 서버로 요청이 들어가던 상관이없습니다
  + 토큰 기반 시스템에서, 토큰에 **선택적인 권한만 부여**하여 발급 가능합니다.
    - ex) 페이스북 계정으로 소셜 로그인을 했다면, 프로필 정보를 가져오는 권한은 있어도 게시글 작성 권한은 제한할 수 있습니다.
  + Scalability: 서버 확장을 의미
* CORS 문제점 해결, 여러 플랫폼 및 도메인 호환 및 많은 종류의 서비스 제공이 가능합니다.

## 단점
stateless한 토큰의 특성 때문에 **토큰을 강제로 만료시킬 수 없는 문제점**이 있습니다. 

토큰이 탈취되었다고 가정하면, 해커는 **토큰이 만료될 때까지 서버에 요청이 가능**합니다. 이러한 문제를 해결하기 위해 토큰의 만료 주기를 짧게하면 수시로 로그인 해야하는 사용자의 불편함이 늘어나는 문제점이 있습니다.

그래서 이를 보완하기 위해 **토큰의 타입을 리프레시 토큰과 액세스 토큰으로 나누는 방식**을 사용합니다.

* **액세스 토큰**: 서비스를 요청하는 데 사용하는 토큰, 만료 주기가 짧음
* **리프레시 토큰**: 액세스 토큰을 재발급받을 수 있는 토큰, 보안이 철저함, 만료 주기가 길다.

만약 액세스 토큰을 탈취당하더라도 리프레시 토큰을 탈취하지 못하면 공격할 수 있는 시간이 많지 않아 피해를 줄일 수 있습니다.

## 토큰 저장 방식
서버가 토큰을 발급해주면, 브라우저에서 **클라이언트-서버 간에 토큰이 전달되는 방식은 크게 2가지**로 나뉩니다.

#### 웹 스토리지(localStorage or sessionStorage)
로그인 성공시 서버가 응답 정보에 토큰을 넣어서 전달하도록 하고, 해당 값을 **웹 스토리지(localStorage 혹은 sessionStorage)에 넣고 다음부터 웹 요청을 할 때마다 HTTP 헤더 값에 넣어서 요청하는 방법**입니다.

이 방법은 **구현하기 쉽고 하나의 도메인에 제한되어있지 않다는 장점**이 있지만, XSS 해킹 공격을 통하여 해커의 악성 스크립트에 노출이 되는 경우 매우 쉽게 토큰이 탈취될 수 있습니다. 그냥 localStorage에 접근하면 바로 토큰에 접근할 수 있기 때문입니다.

#### 쿠키
쿠키를 사용한다고해서 세션을 관리하는 것은 아니고, 그저 **쿠키를 정보 전송수단으로 사용**할 뿐입니다.

이 과정에서 서버측에서 응답을 하면서 쿠키를 설정해 줄 때 httpOnly 값을 활성화를 해주면, 네트워크 통신 상에서만 해당 쿠키가 붙게 됩니다. 따라서 브라우저상에서는 자바스크립트로 토큰 값에 접근하는 것이 불가능해집니다. 그래서, 이 방법의 **단점은** 

* **쿠키가 한정된 도메인에서만 사용이 된다는 점**입니다.
  + 이 문제는 토큰이 필요해질 때 현재 쿠키에 있는 토큰을 사용하여 새 토큰을 문자열로 받아올 수 있게 하는 API를 구현하여 해결하면 됩니다.
* XSS의 위험에서 완벽히 해방되는 대신 **CSRF 공격의 위험성**이 있습니다.
  + httponly, secure flag 세워두면 CSRF 공격을 제외한 취약점에는 안전할 수 있습니다.
  + CSRF는 계정 정보를 탈취하는 것은 아니지만 스크립트를 통해 사이트의 외부에서 사이트의 API를 사용하는 것처럼 모방할 수 있습니다. 또는, 사이트 내부에서 스크립트가 실행되어 원하지 않는 작업이 수행되게 할 수도 있습니다.
  + ex) 유저도 모르는 사이 탈퇴, 덧글을 자동으로 작성, 포스트를 자동으로 작성, 회원정보 변경
  + 이러한 CSRF는 HTTP 요청 레퍼러 체크, CSRF 토큰의 사용을 통하여 방지할 수 있습니다.

보통 쿠키를 사용하는것이 일반적입니다. 이유는 다른 취약점에 비해 CSRF 취약점을 방지하기가 쉽기 때문입니다.

## 토큰 사용 예시
OAuth, JWT(Json Web Token), OpenID 등

> 추후 JWT와 관련된 내용을 다뤄보겠습니다.

## Token vs Cookie(session)

|Token|Cookie(Session)|
|:---:|:---:|
|클라이언트는 서버에 요청할때 토큰을 포함하여 요청하고 서버는 메모리 혹은 데이터베이스 조회없이 암호학 기술로 정상 토큰인지만 확인한다|서버는 요청이 올때마다 메모리 혹은 데이터베이스를 조회하여 확인한다|
|서버에서 세션이 필요하지 않지만 JWT는 하나를 가질 수 있다|서버에 의해서 생성된다|
|세션 정보도 포함되어 있으므로 사용자에 대한 실제 데이터를 포함한다||
|제한된 수명을 가지고 있다||
|데이터 하위 집합에만 엑세스 권한을 부여할 수 있다||

## 결론
세션, 토큰 둘다 같이 병렬로 사용됩니다. 

세션 기반 접근 방식은 웹 사이트를 사용할 때 배포되지만 토큰 기반은 동일한 서비스에서 앱을 사용할 때 선호합니다.

## References
* 컴퓨터네트워킹 하향식 접근 제 7판
* [wikipedia 세션](https://ko.wikipedia.org/wiki/%EC%84%B8%EC%85%98_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))
* [cjh5414: cookie-and-session](https://cjh5414.github.io/cookie-and-session/)
* [devhaks: session의 한계, 문제점](https://devhaks.github.io/2019/04/20/session-strategy/)
* [wikipedia: HTTP_쿠키](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4)
* [youtube: session, cookies, token](https://www.youtube.com/watch?v=44c1t_cKylo&ab_channel=ValentinDespa)
* [hahahoho5915: session, cookie](https://hahahoho5915.tistory.com/32)
* [devuna: session, cookie](https://devuna.tistory.com/23)
* [coyagi: session, cookie secure coding](https://coyagi.tistory.com/entry/%EC%8B%9C%ED%81%90%EC%96%B4%EC%BD%94%EB%94%A9-%EC%BF%A0%ED%82%A4-%EB%B0%8F-%EC%84%B8%EC%85%98%EA%B4%80%EB%A6%AC)
- [dooopark: token](https://dooopark.tistory.com/6)
- [sanghaklee: jwt](https://sanghaklee.tistory.com/47)
- [jwt.io: jwt](https://jwt.io/)
- [rfc7519: jwt](https://tools.ietf.org/html/rfc7519)
- [velopert: jwt](https://velopert.com/2350)