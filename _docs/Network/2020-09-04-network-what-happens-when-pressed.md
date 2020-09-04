---
title:  "google.com을 누르면 생기는 일"
category: Network
date:   2020-09-04 00:30:59
comments: true
order: 4
---


기술 면접에서 단골 출제되는 질문인 url에 www.google.com을 입력하고 눌렀을때 어떤 일이 일어나는지에 대해서 알아보겠습니다.

## google.com을 누르면 생기는 일
<a href="{{ site.baseurl }}{{ site.network_img }}/network-dns-server-interaction.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.network_img }}/network-dns-server-interaction.JPG" title="Check out the image">
</a>

* DNS 서버에서 도메인(google)에 해당하는 IP주소를 요청합니다.
  + 상세하게 보면 먼저 로컬 DNS에서 IP주소를 요청하고 없으면
  + 루트DNS에서 IP주소를 요청하고 없으면
  + TLD(.com) DNS에서 IP주소를 요청하고 없으면
  + ex(naver.com, google.com)을 관리하는 책임 DNS에서 IP주소를 요청합니다.
* 수신한 IP주소에 해당하는 웹 서버에 접속한다.
  + 사용자의 PC는 DHCP서버에서 사용자 자신의 IP주소, 가장 가까운 라우터의 IP주소, 가장 가까운 DNS서버의 IP주소를  받습니다.
  + 이후, ARP를 이용해서 IP주소를 기반으로 가장 가까운 MAC주소를 알아냅니다.
  + 외부와 통신할 준비를 마쳤으므로, DNS Query를 DNS서버에 송신합니다. 
    - DNS 서버는 이에대한 결과로 웹 서버의 IP주소를 사용자 PC에 돌려줍니다.
  + 이제 IP를 알아냈으므로, HTTP Request를 위해 TCP Socket을 개방하고 연결합니다(3way handshaking)
    - 클라이언트가 서버로 연결 요청(SYN플래그)을 보냅니다.
    - 서버가 요청에 응답하며(SYN + ACK) 포트를 개방 요청을 합니다.
    - 클라이언트가 수락하는 응답을 보냅니다(ACK).
  + TCP연결에 성공하면 HTTP Request가 TCP Socket을 통해 보내지고, 응답으로 웹페이지의 정보를 받습니다.

## Reference
* [SantonyChoi/what-happens-when-KR](https://github.com/SantonyChoi/what-happens-when-KR)