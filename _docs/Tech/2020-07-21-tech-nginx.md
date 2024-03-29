---
title: Nginx
category: Tech
date: 2020-07-21 00:30:59
lastmod: 2020-09-30 00:30:59
comments: true
order: 1
---


## 목차
* Nginx 란?
* Event-Driven
* HTTP
* Reverse Proxy
* Apache와 Nginx 비교

## Nginx란?
__Nginx는 가벼움과 높은 성능을 목표__ 로 러시아의 프로그래머, 이고르 시쇼브가 Apache의 C10K Problem(하나의 웹서버에 10,000개의 클라이언트의 접속을 동시에 다룰 수 있는 기술적인 문제)를 해결하기 위해 만들어졌습니다.

* Nginx는 트래픽이 많은 웹사이트를 위해 확장성을 위해 설계한 __비동기 이벤트 기반구조의 웹서버 소프트웨어__ 입니다.
  + 즉, 더 적은 자원으로 더 빠르게 서비스를 제공하기 위한 SW라고 할 수 있습니다.
* Nginx는 Event-Driven구조와 __다음 기술들을__ 제공하는 오픈소스 서버 프로그램입니다.
  + HTTP
  + 로드벨런싱(Reverse Proxy)
  + IMAP/POP PROXY server
  + 분산 메모리에 객체 캐시 시스템

## Event-Driven
![web-nginx-thread-eventdriven-programming]({{ site.baseurl }}/images/Web/web-nginx-thread-eventdriven-programming.JPG)

__Thread 기반__ 은 하나의 커넥션당 하나의 쓰레드를 잡아 먹지만
 
__Event-Driven 방식__ 은 여러개의 커넥션을 전부 Event Handler를 통해 __비동기 방식으로 처리__ 해 먼저 처리되는 것부터 로직이 진행되게끔 합니다.

## HTTP
Nginx는 정적 파일을 처리하는 __HTTP 서버로서의 역할__ 을 합니다.

__웹서버의 역할__ 은 HTML, CSS, Javascript, 이미지와 같은 정적인 정보를 웹 브라우저(Chrome, IE, Firefox 등)에 전송하는 역할을 한다. (HTTP 프로토콜을 준수)

## Reverse Proxy
![web-nginx-reverseproxy]({{ site.baseurl }}/images/Web/web-nginx-reverseproxy.JPG)

__리버스 프록시(reverse proxy)란__, __클라이언트__ 가 가짜 서버에 요청(request)하면, __프록시 서버(Nginx)__ 가 __reverse server(응용프로그램 서버)__ 로 요청을 전달하고 실제 요청에 대한 처리는 뒷단의 reverse server(응용프로그램 서버)들이 처리한 데이터를 가져오는 역할을 한다.

__웹 응용프로그램 서버 에 리버스 프록시(Nginx)를 두는 이유__ 는 요청(request)에 대한 버퍼링이 있기 때문이다. 클라이언트가 직접 App 서버에 직접 요청하는 경우, 프로세스 1개가 응답 대기 상태가 되어야만 한다. 따라서 프록시 서버를 둠으로써 요청을 배분하는 역할을 한다.

## Apache와 Nginx 비교
기존에는 거의 __Apache__ 를 웹서버로 사용했습니다. Apache는 다양한 기능과 서드파티 확장 기능 등 어떠한 웹 애플리케이션에도 적용할 수 있는 웹서버였지만 클라이언트 접속 당 CPU와 메모리 사용량이 증가함으로써 확장성이 떨어진다는 단점이 있었습니다. 그래서 대량의 클라이언트 접속을 관리하기 위한 웹서버가 필요로 했고, 그래서 나온 것이 __Nginx__ 입니다.

#### Apache
* MPM(multi Processing Module: 다중 처리 모듈) 방식을 사용
* 쓰레드/프로세스 기반 구조로 요청 하나당 쓰레드 하나가 처리하는 구조
* 사용자가 많으면 많은 쓰레드 생성, 메모리 및 CPU 낭비가 심함
* __하나의 쓰레드__ : 하나의 클라이언트 라는 구조

#### Nginx
* 비동기 Event-Driven 기반 구조
* 다수의 연결을 효과적으로 처리 가능
* 대부분의 코어 모듈이 Apache보다 적은 리소스로 더 빠르게 동작가능
* 더 작은 쓰레드로 클라이언트의 요청들을 처리가능


## References
* [m.blog.naver.com/jhc9639](https://m.blog.naver.com/jhc9639/220967352282)
* [whatisthenext.tistory.com/123](https://whatisthenext.tistory.com/123)