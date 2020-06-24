---
title:  "TCP와 UDP"
category: Network
date:   2020-06-24 11:30:59
comments: true
order: 3
---

## TCP란?
* TCP 연결형 서비스로 __가상 회선 방식__ 을 제공한다.
  + 가상 회선 방식: 발신지와 수신지를 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다는 말이다.
* 연결형 프로토콜
* __3 hand shake__ 과정을 통해 연결을 설정하고 __4 hand shake__ 을 통해 해제한다.
  + 3 hand shacking과정은 목적지와 수신지를 확실히 하여 정확한 전송을 보장하기 위해서 세션을 수립하는 과정을 의미한다.
* 흐름 제어 및 혼잡 제어
  + 흐름제어는 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것입니다.
    - 송신하는 곳에서 감당이 안되게 데이터를 빠르게 많이 보내면 수신자에서 문제가 발생하기 때문입니다.
  + 혼잡제어는 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것입니다.
    - 만약 정보의 소통량이 과다하면 패킷을 조금만 전송하여 붕괴 현상이 일어나는 것을 막습니다.
* 높은 신뢰성을 보장한다. UDP보다 속도가 느리다.
* __전이중(Full duplex), 점대점(Point to Point) 방식.__
  + 전이중: 전송이 양방향으로 동시에 일어날 수 있다.
  + 점대점: 각 연결이 정확히 2개의 종단점을 가지고 있다.

## TCP connection

![network-tcp_connection]({{ site.baseurl }}/images/Network/network-tcp_connection.JPG)

#### 3 hand shack(TCP connection)
* open()을 실행한 클라이언트가 SYN 보낸다. SYN_SENT 상태로 대기
* 서버는 SYN_RCVD 상태로 바꾸고 SYN과 응답 ACK를 보낸다.
* SYN과 응답 ACK를 받은 클라이언트는 ESTABLISHED 상태로 변경, 서버에게 응답 ACK를 보낸다.
* ACK를 받은 서버는 ESTABLEISHED 상태로 변경
* 요약
  + 클라->서버 : SYN 전송
    - 클라: SYN_SENT, 서버: x
  + 서버->클라 : SYN ACK 전송
    - 클라: SYN_SENT, 서버: SYN_RCVD
  + 클라 ->서버 : ACK 전송
    - 클라: ESTABLISHED, 서버: SYN_RCVD
  + 서버
    - 클라: ESTABLISHED, 서버: ESTABLISHED

#### 4 hand shack(TCP disconnection)
* close()를 실행한 클라이언트가 FIN 보낸다. FIN_WAIT1 상태로 대기
* 서버는 CLOSE_WAIT 상태로 바꾸고 ACK를 보낸다. 동시에 해당 포트에 연결된 어플리케이션에게 close()를 요청
* ACK를 받은 클라이언트는 FIN_WAIT2 상태로 변경
* close()요청을 받은 어플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트에 보내고 LAST_ACK 상태로 변경
* FIN 받은 클라이언트는 ACK를 서버에 전송, TIME_WAIT 상태로 변경. TIME_WAIT에서 일정 시간이 지나면 CLOSED 된다. ACK 받은 서버도 포트를 CLOSED로 닫는다.
* 요약
* 클라->서버 : FIN 전송
  + 클라: FIN_WAIT1, 서버: x
* 서버->클라 : ACK 전송 & 서버->어플 : close()요청
* 어플->클라 : FIN
  + 클라: FIN_WAIT1, 서버: CLOSE_WAIT
* 클라(ACK 받을때)
  + 클라: FIN_WAIT2, 서버: LAST_ACK
* 클라->서버 : ACK (어플->클라 : FIN 받을때)
  + 클라: TIME_WAIT, 서버 LAST_ACK

## TCP의 패킷 추적
데이터는 패킷단위로 나누어 같은 목적지(IP계층)으로 전송된다. 예를들어 ABC가 서울에서 부산으로 순차적으로 가는 상황에서 B가 분실된 상황에서 목적지(부산)은 AC만 보고 다왔다고 착각한다. 그렇기 때문에 ABC라는 패킷에 1,2,3라는 번호를 부여하여 패킷의 분실 확인과 같은 처리를 하여 목적지에서 재조립을 한다.

## UDP란?
* UDP 데이터를 데이터그램 단위로 처리하는 프로토콜
  + 데이터그램이란 독립적인 관계를 지니는 패킷을 뜻한다.
    - 독립적인 관계라는것을 각 패킷은 독립적인 관계를 지니게 되는데 데이터들이 서로 다른 경로로 독립적으로 처리(전송)된다는 의미이다.
* 소켓 대신 ip를 기반으로 데이터를 전송한다.
* 비연결형 프로토콜
* UDP헤더의 checksum 필드를 통해 최소한의 오류만 검출한다.
* 신뢰성이 낮다. TCP보다 속도가 빠르다.
* 연속성이 중요한 실시간 서비스(streaming)에 자주 사용된다.


## References