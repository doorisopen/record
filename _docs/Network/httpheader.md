---
title:  "Http Header"
category: Network
date:   2019-12-05 12:13:59
comments: true
order: 1
---


## __Http 요청 메시지__
```
GET/somedir/page.html HTTP/1.1 200 OK
Host: www.example.com
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
```
* __특징__
1. 메시지가 일반 ASCII 텍스트로 쓰여 있어 사람들이 읽을 수 있다
2. 메시지가 다섯 줄로 되어 있고 각 줄은 CR(carriage return)과 LF(line feed)로 구별된다
3. 마지막 줄에 이어서 추가 CR과 Lf가 따른다  

* Http 요청 메시지의 첫 줄은 __요청 라인(request line)__ 이라 부르고 이 후 줄 들은 __헤더 라인(header line)__ 이라고 부른다

## __헤더 라인(header line)__
* 헤더 라인 __Host: www.example.com__ 는 객체가 존재하는 호스트를 명시하고 있다. 이 헤더 라인이 불필요하다고 생각할 수 있지만 이미 호스트까지 TCP 연결이 맺어 있다
  + 호스트 헤더 라인이 제공하는 정보는 웹 프록시 캐시에서 필요로 한다.
* __Connection: close__ 는 브라우저는 서버에게 지속 연결 사용을 원하지 않는다는 것을 말한다(브라우저는 서버가 요청 객체를 보낸 후에 연결을 닫기를 원함)
* __User-agent: Mozilla/5.0__ 는 사용자 에이전트 즉, 서버에게 요청을 하는 브라우저 타입을 명시하고 있다(Mozilla/5.0는 파이어폭스 브라우저다)
* __Accept-language: fr__ 헤더는 사용자가 객체의 프랑스어 버전(서버에 존재한다면)을 원하고 있음을 의미. 이것이 존재하지 않으면 서버는 기본 버전을 보낸다 이 헤더는 HTTP에서 사용 가능한 많은 콘텐츠 협상 헤더 중 하나다


# References
> * <a href="#">컴퓨터 네트워킹 하향식 제7판<a>