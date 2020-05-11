* checked exception unchecked exception
체크드 익셉션은 try catch로 안감싸면 무조건 빨간줄...
보통은 언체크로 ㅋㅋ 그래야 트랜잭션 할때 롤백을


* 쓰레드세이프

[참신한닉네임] [오후 4:51] 쓰레드 세이프 하기 위해서 어떤걸 하고
[참신한닉네임] [오후 4:52] 어떤 장단점이 있고 ..

* gc의 종류와 방법을 적어라. 자바 버전별로

http://www.yes24.com/Product/Goods/72161685


최근에 기술면접에서 나왔던 질문들을 몇가지 정리해봤어요 그냥 아침에 정보 공유차 정리해봤어요 ㅋㅋ 
1. [ ] static 
2. [ ] 프로세스 스레드 -> 그냥 전 프로세스는 메모리에 올라가는 놈이고 스레드는 프로세스 내에서 하나의 실행 흐름 정도로 답해요.
3. [x] interface abstract
3. [ ] 쓰레드세이프
4. [ ] 자바 동기화 
5. [ ] checked exception unchecked exception
6. [ ] 스프링 요청 처리방법 
7. [ ] dfs bfs 란 , 어디에서 쓰이나 
8. [ ] 맵 해시맵 셋 설명 
9. [ ] presicion recall f1score accuracy 
10. [ ] 스택 영역 힙 영역 설명 
11. [ ] Call By Value 와 Call By Reference의 차이? 왜 그렇게 나눠서 사용하는지? 값 참조와 주소 참조를 구분해서 사용하는 이유?
12. [ ] 힙이랑 스택 차이는 뭔가
13. [ ] 스프링 처리흐름
14. [ ] 데드락
15. [ ] 자바에서 동기화 처리방법
16. [ ] ConcurrentHashMap
17. [ ] Synchronized
18. [ ] hashMap이 왜 thread-safe가 아닌지
19. [ ] Thread.top이 왜 비권장이 되었는지?
20. [ ] 영화좌석 예매 시스템을 짜려고 할때 그 좌석이 예매되었는지 확인하기 위해 어떤 자료구조를 쓸것인가 라는 질문도 한번 받은적있네요
21. [ ] 메모리구조 상 컨텍스트 스위칭이 뭐가 더 효율적인지도 물어봐여 ㅋㅋ 멀티쓰레드일때의 컨텍스트 스위칭과 멀티프로세스일때의 컨텍스트 스위칭과
22. [ ] www.naver.com 입력하고 접속할 때 네트워크 상에서 어떤 과정이 일어나는지??
-->
* DNS 서버에서 도메인에 해당하는 IP주소를 요청한다.
    - 상세하게 보면 먼저 로컬 DNS에서 IP주소를 요청하고 없으면
    - 루트DNS에서 IP주소를 요청하고 없으면
    - .COM DNS에서 IP주소를 요청하고 없으면
    - ex(naver.com, google.com)을 관리하는 DNS에서 IP주소를 요청한다.
* 수신한 IP주소에 해당하는 웹 서버에 접속한다.
    - 사용자의 PC는 DHCP서버에서 사용자 자신의 IP주소, 가장 가까운 라우터의 IP주소, 가장 가까운 DNS서버의 IP주소를 받는다.
    - 이후, ARP를 이용해서 IP주소를 기반으로 가장 가까운 MAC주소를 알아낸다.
    - 외부와 통신할 준비를 마쳤으므로, DNS Query를 DNS서버에 송신한다. DNS 서버는 이에대한 결과로 웹 서버의 IP주소를 사용자 PC에 돌려준다.
    - 이제 IP를 알아냈으므로, HTTP Request를 위해 TCP Socket을 개방하고 연결한다(뭐 대충 요쯤에? 3way handshaking)
        - 클라이언트가 서버로 연결 요청(SYN플래그)을 보낸다.
        - 서버가 요청에 응답하며(SYN + ACK) 포트를 개방해달라한다.
        - 클라이언트가 수락하는 응답을 보낸다(ACK).
    - TCP연결에 성공하면 HTTP Request가 TCP Socket을 통해 보내지고, 응답으로 웹페이지의 정보를 받는다.
23. 3way 핸드쉐이킹시에 2번 단계에서 클라이언트가 서버에 보낸 ack+syn플래그를 못받았을 때 어떻게 되는가? -> 1번에서 클라이언트가 서버로 syn 보내고 시간을 재는데 timeout되기전까지 ack+syn가 안오면 다시 서버로 syn보내고 ack+syn플래그 수신을 대기한다더라고여
24. TCP 연결 과정(3way) vs 종료 고정(4way) 한 단계 차이나는 이유는? [참고](https://goodgid.github.io/TCP-IP-3Way-4Way/#4-way-handshaking-%EA%B3%BC%EC%A0%95)

그냥 아침에 생각나서 정리해봣어요 다들 저처럼 얕게 공부해서 답변 못하지 마시구 취뽀하세요
* 백엔드 면접 질문 리스트
https://oolaf.tistory.com/123

* 프엔 면접 질문 리스트 

https://h5bp.org/Front-end-Developer-Interview-Questions/translations/korean/