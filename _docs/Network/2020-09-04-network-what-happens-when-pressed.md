---
title:  "google.com을 누르면 생기는 일"
category: Network
date: 2020-09-04 00:30:59
lastmod: 2020-10-03 00:30:59
comments: true
order: 4
---


기술 면접에서 단골 출제되는 질문인 url에 www.google.com을 입력하고 눌렀을때 어떤 일이 일어나는지에 대해서 알아보겠습니다.

## google.com을 누르면 생기는 일
<a href="{{ site.baseurl }}{{ site.network_img }}/network-dns-server-interaction.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.network_img }}/network-dns-server-interaction.JPG" title="Check out the image">
</a>

* 컴퓨터를 네트워크에 처음 연결하게 되면 DHCP 서버로부터 IP주소와 TCP/IP 프로토콜의 기본 설정(사용자 자신의 IP주소, 가장 가까운 라우터의 IP주소, 가장 가까운 DNS서버의 IP주소)을 획득하기 위해서 DHCP 프로토콜을 실행합니다.
* 외부와 통신할 준비를 마쳤으므로, DNS Query에 google.com을 넣고 DNS서버(포트:53)에 송신합니다. 
* DNS 서버에서 도메인(google.com)에 해당하는 IP주소를 요청합니다.
  + 상세하게 보면 먼저 로컬 DNS에서 IP주소를 요청하고 없으면
  + 루트DNS에서 IP주소를 요청하고 없으면
  + TLD(.com) DNS에서 IP주소를 요청하고 없으면
  + ex(naver.com, google.com)을 관리하는 책임 DNS에서 IP주소를 요청합니다.
  + DNS 서버는 이에대한 결과로 웹 서버의 IP주소를 사용자 PC에 돌려줍니다.
* 수신한 IP주소에 해당하는 웹 서버에 접속한다.
  + 목적지 게이트웨이의 IP주소는 알지만 MAC 주소는 모른다면 ARP를 이용해서 IP주소를 기반으로 MAC주소를 알아냅니다. 
  + IP와 MAC주소를 알아냈으므로, HTTP Request를 위해 TCP Socket을 개방하고 연결합니다(3way handshaking)
    - 클라이언트가 서버로 연결 요청(SYN플래그)을 보냅니다.
    - 서버가 요청에 응답하며(SYN + ACK) 포트를 개방 요청을 합니다.
    - 클라이언트가 수락하는 응답을 보냅니다(ACK).
  + TCP연결에 성공하면 HTTP Request가 TCP Socket을 통해 보내지고, 응답으로 웹페이지의 정보를 받습니다.

#### DHCP(Dynamic Host Configuration Protocol)
__DHCP(Dynamic Host Configuration Protocol)__ 란 호스트의 IP주소와 각종 TCP/IP 프로토콜의 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜을 말합니다. 

* DHCP에 대한 표준은 RFC문서에 정의되어 있습니다.
* DHCP는 네트워크에 사용되는 IP주소를 DHCP서버가 중앙집중식으로 관리하는 클라이언트/서버 모델을 사용합니다.
* DHCP지원 클라이언트는 네트워크 부팅과정에서 DHCP서버에 IP주소를 요청하고 이를 얻을 수 있습니다.

#### ARP(Address Resolution Protocol)
__ARP(Address Resolution Protocol - 주소 결정 프로토콜)__  는 네트워크 상에서 IP 주소를 물리적 네트워크 주소로 대응(bind)시키기 위해 사용되는 프로토콜입니다. 쉽게 말해, TCP/IP 3계층(네트워크계층)의 IP Address를 2계층(데이터링크계층)의 MAC address로 대응 시킬때 사용하는 프로토콜 입니다.

여기서 물리적 네트워크 주소는 이더넷 또는 토큰링의 48 비트 네트워크 카드 주소를 뜻한다. ARP는 1982년 인터넷 표준 STD 37인 "RFP 826"에 의해 정의되었습니다.

예를 들어, IP 호스트 A가 IP 호스트 B에게 IP 패킷을 전송하려고 할 때 IP 호스트 B의 물리적 네트워크 주소를 모른다면, ARP 프로토콜을 사용하여 목적지 IP 주소 B와 브로드캐스팅 물리적 네트워크 주소 FFFFFFFFFFFF를 가지는 ARP 패킷을 네트워크 상에 전송합니다. IP 호스트 B는 자신의 IP 주소가 목적지에 있는 ARP 패킷을 수신하면 자신의 물리적 네트워크 주소를 A에게 응답합니다.

이와 같은 방식으로 수집된 IP 주소와 이에 해당하는 물리적 네트워크 주소 정보는 각 IP 호스트의 ARP 캐시라 불리는 메모리에 테이블 형태로 저장된 다음, 패킷을 전송할 때에 다시 사용됩니다. ARP와는 반대로, IP 호스트가 자신의 물리 네트워크 주소는 알지만 IP 주소를 모르는 경우, 서버로부터 IP주소를 요청하기 위해 RARP를 사용합니다.

## Reference
* [SantonyChoi/what-happens-when-KR](https://github.com/SantonyChoi/what-happens-when-KR)
* [개발자를 꿈꾸는 프로그래머](https://jwprogramming.tistory.com/35)
* [wikipedia.org/wiki/arp](https://ko.wikipedia.org/wiki/%EC%A3%BC%EC%86%8C_%EA%B2%B0%EC%A0%95_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
* [blockdmask-개발자 지망생](https://blockdmask.tistory.com/189)
* [owlgwang.tistory.com/1](https://owlgwang.tistory.com/1)