# story13. XML과 JSON도 잘 쓰자
생성일: 2022년 6월 28일 오전 12:02

# Java의 XML 파서

XML(eXtensible Markup Language)

> 마크업 언어(Markup Language)란 태그 기반의 텍스트로 된 언어를 의미한다. 태그 안에 필요한 데이터를 추가함으로써 데이터를 전달하거나 보여주는 것이 주 목적이다.
> 

| 약어 | 의미 | 패키지 | 비고 |
| --- | --- | --- | --- |
| JAXP | Java API for XML Processing | javax.xml.parsers |  |
| SAX | Simple API for XML | org.xml.sax | 서버에서 작업하기 적합 |
| DOM | Document Object Model | org.w3c.dom | 서버에서 작업하기 적합 |
| XSLT | Xml Stylesheet Language for Transformations | javax.xml.transform |  |
- SAX는 순차적 방식으로 XML을 처리한다
    - 메모리에 부담이 DOM에 비해 많지 않다.
    - 이미 읽은 데이터의 구조를 수정하거나 삭제하기 어렵다.
- DOM은 모든 XML을 읽어서 트리(Tree)를 만든 후 XML을 처리하는 방식이다.
    - 모든 XML을 메모리에 올려서 작업하기 때문에 메모리에 부담이 있다.
    - 노드 추가, 수정, 삭제가 쉬운 구조이다.
- XSLT는 SAX, DOM, InputStream을 통해서 들어온 데이터를 원하는 형태의 화면으로 구성하는 작업을 수행한다.(XML이 화면에서 보기 쉬운 데이터가 되도록 처리하는 것)

# JSON

**JSON 파서 종류**

- jackson JSON
- google-gson

> **Serialize**는 데이터를 전송할 수 있는 상태로 처리하는 것
**Deserialize**는 전송 받은 데이터를 사용 가능한 상태로 처리하는 것
> 

# 데이터 전송을 빠르게 하는 라이브러리

xml이나 json을 사용하여 데이터를 전송하는 방식은 많이 사용해 왔지만, 요즘에는 **자바 객체를 전송하는 방식을 많이 사용**한다.

- protobuf: 구글 오픈소스 라이브러리
- Thrift: 페이스북 Apache 프로젝트로 오픈된 라이브러리
- avro

[https://martin.kleppmann.com/2012/12/05/schema-evolution-in-avro-protocol-buffers-thrift.html](https://martin.kleppmann.com/2012/12/05/schema-evolution-in-avro-protocol-buffers-thrift.html)