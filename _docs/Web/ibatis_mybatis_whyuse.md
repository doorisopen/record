---
title: iBatis와 myBatis를 왜 사용하는가??
category: Web
comments: true
order: 6
---

# iBatis와 myBatis를 왜 사용하는가??

## 가장 큰 이유는 빠른 개발(생산성)이 가능하다. 
* DBCP만을 썼을때 Connection, ResultSet, Statement, Transaction 관리도 해야 하고 
* 특히 운영하다 명시적인 Connection, ResultSet, Statement, Transaction을 잘못(닫질 않아) 써서 서버가 죽는경우 허다합니다. 
* ResultSet의 데이터 매핑도 신경써야 하고
* 소스코드가 하드코딩(""+""+"")되어 있음 소스 분석 및 관리가 힘듭니다. (10,000 라인 넘어가는 소스 한번 보믄 이해하실겁니다. )

## 보안적으로 SQL Injection 보안에 신경쓰지 않아도 된다. 
* DBCP를 쓸 경우 PrepareStatement를 쓰면 문제 없다. 

## RDBMS가 Oracle일 경우 Blob, Clob 치환에 신경쓰지 않아도 됩니다. 

### 참고
* __LOB__
  + LOB은 TEXT, 그래픽, 이미지, 비디오, 사운드 등 구조화되지 않은 대형 데이터를 저장하는데 사용한다.
  + 일반적으로 테이블에 저장되는 구조화된 데이터들은 크기가 작지만, 멀티미디어 데이터는 크기가 크다.
  + 크기가 큰 데이터는 DB에 저장하기 힘들기 때문에 OS상 존재하는 파일을 데이터베이스가 접근하게 된다.
  + LONG, LONG RAW 데이터 유형은 예전에 사용던 것이고, 현재는 대부분 LOB 데이터 유형을 사용한다.
  + TO_LOB 함수를 이용하여 LONG 및 LONG RAW 를 LOB 으로 변경할 수 있다.
* __종류__
  + CLOB: 문자 대형 객체 (Character). Oracle Server는 CLOB과 VARCHAR2 사이에 암시적 변환을 수행한다.
  + BLOB: 이진 대형 객체 (Binary). 이미지, 동영상, MP3 등... 
  + NCLOB: 내셔널 문자 대형 객체 (National). 오라클에서 정의되는 National Character Set을 따르는 문자.
  + BFILE: OS에 저장되는 이진 파일의 이름과 위치를 저장. 읽기 전용 모드로만 액세스 가능.


## 디버깅이 쉬워진다.
* iBatis, myBatis 쿼리문의 ? => value로 매핑된 쿼리문으로 로그를 남기는 jar파일이 여럿 있습니다. 

## 데이터 캐싱(LIFO, FIFO, LRU)이 가능하다.
* 조회용 데이터 성능이 안나올 때 데이터 캐싱 설정만으로 훌륭한 성능 개선을 할 수 있습니다. 
* 실제 대규모 사이트에서 적용 사용하고 있음. 
* 간단한 xml 설정만으로 적용 가능 

## 꼭 resultType, resultClass로 VO를 사용 않해도 된다. Map으로만 이용 가능 


# References
* [okky.kr/article/285215](https://okky.kr/article/285215)
* [디딤돌](https://stepping.tistory.com/30)