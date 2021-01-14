---
title: REST API 설계 가이드
category: Web
date:   2020-06-04 00:30:59
lastmod: 2021-01-14 00:30:59
comments: true
order: 5
---

> __REST API를 좀 더 RESTful 하게 설계하기 위한 가이드__


## 목차
* REST 아키텍쳐를 제대로 사용하는 것?
* URI을 보고 직관적으로 이해할 수 있어야한다.
  + 최대 2 depth 정도로 간단하게 만든다.
  + 리소스명은 동사보다는 명사를 사용한다.
    + 리소스명은 소문자를 사용한다.
    + 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용
  + underbar(_) 대신 dash(-)를 사용한다.
  + 마지막에 / 포함하지 않는다.
* 리소스간의 관계를 표현하는 방법.
  + 서브 리소스로 표현하기
  + 서브 리소스에 관계를 명시하기
* HTTP Headers
  + Content-Location
  + Content-Type
  + Retry-After
  + Link
* HATEOAS
* 에러 처리
  + 의미에 맞는 HTTP status를 리턴한다
  + HTTP status만으로 상태 에러를 나타낸다
  + 5XX 에러는 절대 사용자에게 노출하지 마라
* 페이징 처리
  + Partial Response
  + HTTP Header의 Link 속성으로 링크 처리
  + HATEOAS를 이용한 링크 처리
  + Link와 HATEOAS 모두 이용한 링크 처리
* 검색
  + 전역 검색과 지역 검색
* API 버전 관리


## REST 아키텍쳐를 제대로 사용하는 것?
HTTP + JSON 조합을 사용했다고 해서 REST라고 하는것은 잘못된 이해중의 하나입니다. 그렇다면 REST 아키텍쳐를 제대로 사용한다는 것은 무엇일까??

* 리소스를 제대로 정의하고 이에대한 CRUD를 HTTP Method인 GET,POST,PUT,DELETE에 맞춰 사용한다.
* 에러코드에 대해서 HTTP Response code(200,404,...)를 사용한다.
* REST에 대한 속성(HATEOS 등)을 제대로 이해하고 디자인 한다.

필자가 알기론 REST를 완벽하게 지키는것은 아주 힘들다고 알고 있지만, 위와 같이 사용했다면 REST라고 말할 수 있을 것 같습니다.

## URI을 보고 직관적으로 이해할 수 있어야한다.
#### 최대 2 depth 정도로 간단하게 만든다.
API를 URI만 보고도, 직관적으로 이해할 수 있어야합니다. 따라서 최대 2 depth 정도로 만드는 것이 이해하기 좋습니다.

```js
/items/
/items/1
```

#### 리소스명은 동사보다는 명사를 사용한다.
URI에 리소스명은 동사보다 명사를 사용합니다.

REST API는 리소스에 대해서 행동을 정의하는 형태를 사용합니다. 예를들어

```js
POST /items
```

는 /items 라는 리소스를 생성하라는 의미입니다. 이는 HTTP Method에 의해 CRUD의 대상이 되는 명사여야 합니다. 다른 예시로

```js
//==Bad==//
POST /setItems
GET /getItems
```

Bad 예시의 경우 리소스가 set/get 등의 행위를 붙인 경우로 좋지 않은 예시 입니다.

그리고 가급적이면 리소스는 **소문자를 사용**하고 의미상 **단수형 명사(/item) 보다는 복수형 명사(/items)를 사용**하는 것이 의미상 표현하기가 좋습니다.

참고로 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용합니다. 함수처럼, 컨트롤 리소스를 나타내는 URL은 동작을 포함하는 이름을 허용합니다.

```js
//==Bad==//
/items/duplicating

//==Good==//
/items/duplicate
```

다음은 일반적으로 권고되는 디자인입니다.

|리소스|POST|GET|PUT|DELETE|
|:---:|:---:|:---:|:---:|:---:|
|/items|새로운 item 생성|items 목록 리턴|bulk로 여러 items 정보 업데이트|모든 items 정보 삭제|
|/items/1|Error|1이라는 Id의 items 정보를 리턴|1이라는 Id의 items 정보를 업데이트|1이라는 Id의 items 정보를 삭제|


#### _(underbar) 대신 -(dash)를 사용한다.
* __-(dash)의 사용도 최소한으로 설계한다.__ 정확한 의미나 표현을 위해 단어의 결합이 불가피한 경우 반드시 -(dash) 사용한다.

```http
//==Bad==//
http://restapi.test.com/items/my_comments

//==Good==//
http://restapi.test.com/items/my-comments
```

#### 마지막에 / 포함하지 않는다.

```http
//==Bad==//
http://restapi.test.com/items/

//==Good==//
http://restapi.test.com/items
```

## 리소스간의 관계를 표현하기
리소스간의 서로 연관관계가 있을 수 있습니다. 쇼핑몰을 예를들면 등록된 상품의 리뷰 목록 등이 예가 될 수 있는데, 상품-리뷰 등과 같이 각각의 리소스간의 관계를 표현하는 방법이 있습니다.

#### 서브 리소스로 표현하기
예를 들어 **"등록된 상품들 중 1번 상품의 리뷰 목록"**을 표현하면

```js
ex) /{리소스 명}/{리소스 Id}/{연관관계가 있는 리소스 명}

GET /items/1/reviews
```

#### 서브 리소스에 관계를 명시하기
만약 관계의 이름이 복잡하다면 관계명을 명시적으로 표현하는 방법이 있습니다. 예를 들어 "spring이라는 이름을 가진 사용자가 찜(좋아요)한 상품 목록"을 표현하면

```js
ex) GET /users/{userId}/likes/items

GET /users/spring/likes/items
```

리소스간의 관계를 표현하는 2가지 방법 어떤 것을 사용해도 문제는 없습니다.

* 첫 번째 방법의 경우 소유 "has"의 관계를 묵시적으로 표현할 때 좋습니다. 
* 두 번째 방법은 관계의 명이 애매하거나 구체적인 표현이 필요할 때 사용합니다.

## HTTP Headers
#### Content-Location
* 요청의 응답 헤더에 새로 생성된 리소스를 식별할 수 있는 __Content-Location 속성__ 을 이용한다.
* HATEOAS로 Content-Location를 대체할 수 있다.

```http
HTTP/1.1 200 OK
Content-Location: /users/1
```

#### Content-Type
* __application/json__ 사용
* application/xml 등을 제공해서 응답 포맷을 이원화 X, 응답 포맷을 여러 개로 나누면 요청 포맷도 나눠야 한다.

#### Retry-After
* 비정상적인 방법(DoS, Brute-force attack)으로 API 서버를 이용하려는 경우 `429 Too Many Requests` 오류 응답과 함께 일정 시간 뒤 요청할 것을 나타낸다.

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
```

#### Link

> 고유 한 URL을 구성하는 대신 링크 헤더 값을 사용하여 호출을하는 것이 중요합니다.

```http
Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next",
<https://api.github.com/user/repos?page=50&per_page=100>; rel="last"
```

| <center>이름</center> |  <center>기술</center> |
|:--------:|:--------:|
| next | 바로 다음 페이지 결과에 대한 링크 관계|
| last | 결과의 마지막 페이지에 대한 링크 관계|
| first | 결과의 첫 페이지에 대한 링크 관계|
| prev | 바로 이전 결과 페이지에 대한 링크 관계입니다|

## HATEOAS
* REST API가 아닌 HTML 환경에선 눈에 보이는 화면이 있기 때문에 __POST /users__ 후에 사용자의 상태가 전이될 수 있는 link를 화면에서 제공할 수 있습니다.
* 이 문제를 해결하기 위해 응답 객체에 해당 리소스의 상태가 전이될 수 있는 link들을 함께 제공한다. link들을 통해 리소스의 다음 상태 전이 정보를 동적으로 제공합니다.

```json
{
  "rel": "self",
  "href": "http://restapi.test.com/users/1",
  "method": "GET"
}
```

## 에러 처리
에러처리는 기본적으로 HTTP Response Code를 사용한 후, Response body에 error detail을 서술하는 것이 좋습니다.

성공 응답은 2XX로 실패 응답은 4XX로 응답합니다.

|Response Code|Description|
|:---:|:---:|
|200|OK|
|201|Created|
|202|Accepted|
|204|No-Content|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|405|Method Not Allowed|
|409|Conflict|
|429|Too Many Requests|

그리고 에러처리에서 **에러 내용에 대한 디테일 내용을 http body에 정의**해서, 상세한 에러의 원인을 전달하는 것이 디버깅에 유리합니다.

```js
ex)
HTTP/1.1 404 Not Found
{
  "message": "Not Found",
  "code": 1004,
  "more info": "https://restapi.test.com/items/1"
}
```

#### 의미에 맞는 HTTP status를 리턴한다

```js
//==Bad==//
HTTP/1.1 200 OK
{
  "result" : false
  "status" : 400
}

//==Good==//
HTTP/1.1 400 Bad Request
{
  "msg" : "check your parameter"
}
```

#### HTTP status만으로 상태 에러를 나타낸다
* 세부 에러 사항은 응답 객체에 표시하거나, 해당 에러를 확인할 수 있는 link를 표시한다. 
* http 상태 코드를 응답 객체에 중복으로 표시할 필요 없다.

```js
//==Bad==//
HTTP/1.1 404 Not Found
{
  "code" : 404,
  "error_code": -765
}

//==Good==//
HTTP/1.1 404 Not Found
{
  "code" : -765,
  "more_info" : "https://restapi.test.com/errors/-765"
}
```

#### 5XX 에러는 절대 사용자에게 노출하지 마라
* API Server level에선 500 에러가 나선 안된다. (서비스 장애가 발생한 것이기 때문)
* 즉, API Server는 모든 발생 가능한 에러를 핸들링해야 한다.
* 만약 API Server를 서빙하는 웹서버(apache, nginx)가 오류일 때는 500 가능


## 페이징 처리
큰 사이즈의 리스트 형태의 응답을 처리하기 위해서는 **페이징 처리**와 **Partial Response 처리**가 필요합니다.

만약 리턴되는 리스트의 크기가 100만개 일때, HTTP Response로 처리하는 것은 서버 성능, 네트워크 비용도 문제도 있지만 딱 봐도 비효율적이기 떄문에 페이징을 고려하는 것이 중요합니다. 페이징은 아래와 같은 스타일을 사용합니다.

```js
Facebook API style: /record?offset=100&limit=25
```

위의 형태는 100번째 레코드에서부터 25개의 레코드를 출력한다는 의미입니다.

#### Ordering
* Collection(리스트)에 대한 GET 요청의 경우(GET /users) 리스트를 클라이언트의 요청에 맞게 정렬해 응답한다.
* __order라는 key를 사용__ 한다.
  + 오름차순: key
  + 내림차순: -key

```js
GET /users?order=name

?order=-name (name 내림차순, name desc)
?order=-name,level (name 내림차순, level 오름차순, name desc, level asc)
```

#### Filtering
* Collection(리스트)에 대한 GET 요청의 경우(GET /users) 리스트 검색 조건을 요청할 수 있다.
  + AND, OR
  + '=', '!='
  + '>', '>='
  + '<', '>='
  + IN(OR), NOT IN
  + LIKE(include)

#### Field-Selecting

```js
//==Request==//
GET /users?fields=level

//==Result==//
HTTP/1.1 200 OK
{
  "level": 10
}
```

* __include:__ ?fields=id,name
* __exclude:__ ?-fields=level
  + level 제외 모두 반환

#### Partial Response
리소스에 대한 응답 메세지에 대해서 모든 필드를 포함할 필요가 없는 케이스가 있습니다. 예를들어 게시판의 경우 사용자Id, 이름, 내용, 날짜, 좋아요, 댓글 등 여러 정보를 가지는데, API를 요청하는 Client의 용도에 따라 선별적으로 몇가지 필드만이 필요한 경우가 있습니다.

필드를 제한하는 것은 전체 응답의 양을 줄여서 네트워크 대역폭(특히 모바일에서) 절약할 수 있고, 응답 메세지를 간소화하여 파싱등을 간략화할 수 있습니다.

```http
Facebook style: /seoul/schools?fields=name,location
```

#### HTTP Header의 Link 속성으로 링크 처리
HATEOAS를 API에 적용하게 되면, Self-Descriptive 특성이 증대되어 API에 대한 가독성이 증가되는 장점을 가지고 있기는 하지만, 응답 메세지가 다른 리소스 URI에 대한 의존성을 가지기 때문에, 구현이 다소 까다롭다는 단점이 있다.

요즘은 Spring과 같은 프레임웍에서 프레임웍 차원에서 HATEOAS를 지원하고 있으니 참고하기 바란다.


```http
HTTP/1.1 200 OK
Link:
<https://restapi.test.com/users?offset=10&limit=10>; rel="next",
<https://restapi.test.com/users?offset=50&limit=10>; rel="last",
<https://restapi.test.com/users?offset=0&limit=10>; rel="first",
<https://restapi.test.com/users?offset=0&limit=0>; rel="prev",
[
  {1, ...},
  {2, ...},
  ...
  {10,...},
]
```

#### HATEOAS를 이용한 링크 처리

```http
HTTP/1.1 200 OK
[
{1, ...},
{2, ...},
...
{10,...},
  "links": [
    {
      "rel": "next",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=10&limit=10
    },
    {
      "rel": "last",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=50&limit=10
    },
    {
      "rel": "first",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=0&limit=10
    },
    {
      "rel": "prev",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=0&limit=0
    },
  ]
]
```

#### Link와 HATEOAS 모두 이용한 링크 처리

```http
HTTP/1.1 200 OK
Link:
  <https://restapi.test.com/users?offset=10&limit=10>; rel="next",
  <https://restapi.test.com/users?offset=50&limit=10>; rel="last",
  <https://restapi.test.com/users?offset=0&limit=10>; rel="first",
  <https://restapi.test.com/users?offset=0&limit=0>; rel="prev",
[
  {1, ...},
  {2, ...},
  ...
  {10,...},
  "links": [
    {
      "rel": "next",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=10&limit=10
    },
    {
      "rel": "last",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=50&limit=10
    },
    {
      "rel": "first",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=0&limit=10
    },
    {
      "rel": "prev",
      "method": "GET",
      "link": "https://restapi.test.com/users?offset=0&limit=0
    },
  ]
]
```

## 검색
검색은 일반적으로 HTTP GET에서 Query String에 검색 조건을 정의하는 경우가 일반적입니다. 이 경우 검색조건이 다른 Query String과 섞여 버릴 수 있습니다. 예를 들면 이름이 kim 이고 지역이 seoul인 사용자를 검색하면 다음과 같이 표현할 수 있습니다.

```http
/user?name=kim&region=seoul
```

여기에 페이징 처리를 추가하면 다음과 같습니다.

```http
/user?name=kim&region=seoul&offset=1&limit=10
```

위의 경우 검색 조건을 구분하기 어려움이 있습니다. 그래서 Query 조건은 하나의 Query String으로 정의하는 것이 좋습니다.

```http
/user?q=name%3Dkim,region%3Dseoul&offset=1&limit=10
```

위와 같이 검색 조건을 URLEncode를 사용해서 "q=name%3kim,region%3seoul"(q=name=kim&region=seoul) 처럼 표현하고 Deleminator를 , 등을 사용하게 되면 검색 조건은 다른 Query 스트링과 분리됩니다. (검색 조건은 서버에서 토큰 단위로 파싱합니다)

#### 전역 검색과 지역 검색
검색 범위에 따라서 고려해야 할 점이 있습니다. 전역 검색은 전체 리소스에 대한 검색을, 지역 검색은 특정 리소스에 대한 검색을 정의합니다.

예를 들어 시스템에 items, members와 같은 리소스가 정의되어 있을때, name='spring'인 리소스에 대한 전역 검색은

```http
/search?q=name%3Dspring
```

위와 같이 정의할 수 있습니다. /search와 같은 전역 검색 URI를 사용합니다. 반대로 지역 검색의 경우 다음과 같이 정의할 수 있습니다.

```http
/item?q=name%3Dspring
```

## API 버전 관리
* URI Versioning을 채택하고, 버저닝 정보는 host레벨이 아닌 path레벨에 명시합니다.
* 예외적으로 서비스의 기본 도메인이 __3차인 경우 path level에 모두 명시합니다.__

```http
//==Bad==//
http://restapiv1.test.com

//==Good==//
{service name}/{version}/{REST URL}
http://restapi.test.com/{service name}/api/v1.0/items
```

이와 같은 형태로 관리하는 이유는, 서비스의 배포 모델과 관계가 있습니다. 

자바 애플리케이션의 경우, {service-name}.v1.0.war, {service-name}.v2.0.war와 같이 다른 war로 배포하여 버전별로 배포 바이너리를 관리합니다. 앞단에 서비스 명을 별도의 URL로 떼어 놓는 것은 향후 서비스가 확장되었을 경우에 해당 서비스만 별도의 서버로 분리해서 배포하는 경우를 생각할 수 있습니다. 외부로 제공되는 URL은 api.server.com/{service-name}/v1.0/items로 하나의 서버를 가르키지만, 내부적으로 reverse proxy를 이용해서 이런 URL을 맵핑할 수 있는데, {service-name}.server.com/v1.0/items 와 같이 매핑하도록 하면, 외부에 노출되는 URL 변경이 없이 향후 확장되었을때 서버를 물리적으로 분리하기 편리합니다.

## Reference
* [이상학의 개발블로그](https://sanghaklee.tistory.com/57)
* [HTTP status](https://sanghaklee.tistory.com/61)
* [github-pagination](https://developer.github.com/v3/#pagination)
* [REST API bcho.tistory](https://bcho.tistory.com/954)
* [GitHub REST API](https://developer.github.com/v3/#pagination)
* [HTTP methods: OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)
* [HTTP methods: HEAD](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD)
* [HTTP methods: PATCH를](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)