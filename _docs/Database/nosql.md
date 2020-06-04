---
title: NoSQL
category: DataBase
comments: true
order: 3
---

## NoSQL 데이터베이스란?
NoSQL 데이터베이스는 특정 데이터 모델에 대해 특정 목적에 맞추어 구축되는 데이터베이스로서 현대적인 애플리케이션 구축을 위한 유연한 스키마를 갖추고 있습니다. NoSQL 데이터베이스는 개발의 용이성, 기능성 및 확장성을 널리 인정받고 있습니다.

## NoSQL(비관계형) 데이터베이스의 작동 방식
NoSQL 데이터베이스에서는 데이터의 액세스 및 관리를 위해 다양한 데이터 모델을 사용합니다. 이러한 데이터베이스 유형은 큰 데이터 볼륨, 짧은 지연 시간과 유연한 데이터 모델이 필요한 애플리케이션에 최적화되었으며, 이는 다른 데이터베이스의 데이터 일관성 제약 일부를 완화함으로써 이루어집니다.

간단한 서적 데이터베이스를 위한 스키마 모델 구축 사례를 고려해 봅시다:

* __관계형 데이터베이스에서,__ 서적 레코드는 흔히 숨겨져(또는 "정규화되어") 별도의 테이블에 보관되고, 관계는 기본 및 외래 키 제약 조건으로 정의됩니다. 이 예시에서 서적 테이블에는 ISBN, 책 제목, 및 에디션 번호 열이 있으며, 저자 테이블에는 저자 ID 및 저자명 열이 있고, 마지막으로 저자-ISBN 테이블에는 저자 ID 및 ISBN 열이 있습니다. 관계형 모델은 중복성을 줄이도록 정규화되고 일반적으로 저장에 최적화된 데이터베이스에서 __데이터베이스가 테이블 사이에서 참조 무결성을 실현할 수 있도록 고안__ 되었습니다.
* __NoSQL 데이터베이스에서,__ 서적 레코드는 보통 JSON 문서로 저장됩니다. 각각의 서적에 대해, 항목, ISBN, 책제목, 에디션 번호, 저자명, 및 저자 ID가 단일 문서 내에 속성으로 저장됩니다. 이 모델에서, 데이터는 직관적 개발과 수평 확장성에 최적화됩니다.

## NoSQL 데이터베이스를 사용해야 하는 이유
NoSQL 데이터베이스는 탁월한 사용자 경험을 제공하기 위하여 유연성과 확장성을 비롯해 고성능의 매우 기능적인 데이터베이스를 필요로 하는 모바일, 웹이나 게이밍과 같은 다양한 현대적인 애플리케이션에 적합합니다.

* __유연성:__ NoSQL 데이터베이스는 일반적으로 유연한 스키마를 제공하여 보다 빠르고 반복적인 개발을 가능하게 해줍니다. 이같은 유연한 데이터 모델은 NoSQL 데이터베이스를 반정형 및 비정형 데이터에 이상적으로 만들어 줍니다.
* __확장성:__ NoSQL 데이터베이스는 일반적으로 고가의 강력한 서버를 추가하는 대신 분산형 하드웨어 클러스터를 이용해 확장하도록 설계되었습니다. 일부 클라우드 제공자들은 완전관리형 서비스로서 이런 운영 작업을 보이지 않게 처리합니다.
* __고성능:__ NoSQL 데이터베이스는 특정 데이터 모델 및 액세스 패턴에 대해 최적화되어 관계형 데이터베이스를 통해 유사한 기능을 충족하려 할 때보다 뛰어난 성능을 얻게 해줍니다.
* __고기능성:__ NoSQL 데이터베이스는 각 데이터 모델에 맞춰 특별히 구축된 뛰어난 기능의 API와 데이터 유형을 제공합니다.


## SQL (관계형) vs. NoSQL(비관계형) 데이터베이스 비교
수십 년간, 애플리케이션 개발을 위해 가장 많이 사용된 데이터 모델은 Oracle, DB2, SQL Server, MySQL, PostgreSQL과 같은 관계형 데이터베이스에서 사용하는 관계형 데이터 모델이었습니다. 2000년대 중반에서 말에 이르러서야 다른 데이터 모델들이 채택되고 사용되는 현상이 눈에 띄기 시작했습니다. 이러한 새로운 데이터베이스와 데이터 모델 등급을 차별화하고 분류하기 위해 "NoSQL"이란 용어가 만들어졌습니다. 흔히 "NoSQL"이란 용어는 "비관계형"과 같은 의미로 사용됩니다.


다양한 기능을 가진 여러 유형의 NoSQL 데이터베이스가 있지만, 다음 표에서는 SQL과 NoSQL 데이터베이스의 몇 가지 차이점에 대해 보여줍니다.

|   |  <center>관계형 데이터베이스</center> |  <center>NoSQL 데이터베이스</center> |
|:--------:|:--------:|:--------:|
| 최적의 워크로드 | 관계형 데이터베이스는 일관성이 뛰어난 온라인 트랜잭션 프로세싱(OLTP) 애플리케이션을 위해 설계되어 온라인 분석 프로세싱(OLAP)에 적합합니다. | NoSQL 데이터베이스는 낮은 지연 시간의 애플리케이션을 포함한 수많은 데이터 액세스 패턴에 맞도록 설계되었습니다. NoSQL 검색 데이터베이스는 반정형 데이터에서 분석을 위해 설계되었습니다  |
| 데이터 모델 | 관계형 모델은 데이터를 행과 열로 구성된 테이블로 정규화합니다. 스키마는 테이블, 행, 열, 인덱스, 테이블 간 관계, 기타 데이터베이스 요소를 정확하게 규정합니다. 데이터베이스는 테이블 사이의 관계에서 참조 무결성을 실현합니다.  | NoSQL 데이터베이스는 키-값, 문서, 그래프 등 성능과 규모 확장에 최적화된 다양한 데이터 모델을 제공합니다.  |
| ACID 속성 | 관계형 데이터베이스는 원자가, 일관성, 격리성 및 지속성(ACID, atomicity, consistency, isolation, and durability)의 속성을 제공합니다: <br/> __원자성__ 는 완벽하게 실행하거나 혹은 전혀 실행하지 않는 트랜잭션을 필요로 합니다. <br/> __일관성__ 은 트랜잭션이 커밋되면 데이터가 데이터베이스 스키마를 준수하도록 요구합니다. <br/> __격리성__ 은 동시에 일어나는 트랜잭션들이 각기 별도로 실행되어야 함을 의미합니다. <br/> __내구성__ 은 예기치 못한 시스템 장애 또는 정전 시 마지막으로 알려진 상태로 복구하는 기능을 필요로 합니다. | NoSQL 데이터베이스는 흔히 수평으로 확장할 수 있는 보다 유연한 데이터 모델을 위해 관계형 데이터베이스의 일부 ACID 속성을 완화함으로써 조정합니다. 이로써 NoSQL 데이터베이스는 단일 인스턴스의 한계를 넘어 수평으로 확장해야 하는 사용 사례에서 높은 처리량, 낮은 지연 시간을 위한 탁월한 선택이 됩니다. |
| 성능 | 성능은 일반적으로 디스크 하위 시스템에 따라 다릅니다. 최고 성능을 달성하기 위해서는 쿼리, 인덱스 및 테이블 구조를 자주 최적화해야 합니다. | 성능은 일반적으로 기본 하드웨어 클러스터 크기, 네트워크 지연 시간 및 호출 애플리케이션의 기능입니다. |
| 확장 | 관계형 데이터베이스는 일반적으로 하드웨어의 계산 성능을 높이거나 읽기 전용 워크로드의 복제물을 추가함으로써 확장됩니다. | NoSQL 데이터베이스는 일반적으로 거의 무제한적인 범위에서 일관된 성능을 제공하는 처리량 제고를 위해 분산형 아키텍처를 사용해 액세스 패턴이 확장 가능하기 때문에 분할성이 있습니다. |
| API | 데이터를 저장 및 검색하기 위한 요청은 SQL(구조화 질의 언어)을 준수하는 쿼리를 사용하여 전달됩니다. 쿼리는 관계형 데이터베이스에 의해 구문 분석되고 실행됩니다. | 객체 기반 API를 통해 앱 개발자가 데이터 구조를 쉽게 저장 및 검색할 수 있습니다. 파티션 키를 사용하면 앱에서 키-값 페어, 열 세트 또는 일련의 앱 객체 및 속성을 포함하는 반정형 문서를 검색할 수 있습니다. |


## SQL과 NoSQL 용어 비교

| <center>SQL</center> |  <center>MongoDB</center> |  <center>DynamoDB</center> |  <center>Cassandra</center> |  <center>Couchbase</center> |
|:--------:|:--------:|:--------:|:--------:|:--------:|
| 테이블 | 컬렉션 | 테이블 | 테이블 | 데이터 버킷 |
| 열 | 문서 | 항목 | 열 | 문서 |
| 컬럼 | 필드 | 속성 | 컬럼 | 필드 |
| 기본 키 | ObjectId | 기본 키 | 기본 키 |  문서 ID |
| 인덱스 | 인덱스 |	보조 | 인덱스 |	인덱스 | 인덱스 |
| 보기 | 보기 | 글로벌 보조 인덱스 | 구체화된 보기 | 보기 |
| 중첩된 | 테이블  또는 객체 | 포함 문서 |	맵 | 맵 | 맵 |
| 배열 | 배열 | 목록 | 목록 | 목록 |

## Reference
[출처: AWS/nosql](https://aws.amazon.com/ko/nosql/)