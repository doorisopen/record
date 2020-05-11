> __REST API를 좀 더 RESTful 하게 설계하기 위한 가이드__


## 1. URL Rules
* 마지막에 / 포함하지 않는다.
* _(underbar) 대신 -(dash)를 사용한다.
* 소문자를 사용한다.
* 행위(method)는 URL에 포함하지 않는다.
* 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.

### 마지막에 / 포함하지 않는다.

```
//==Bad==//
http://restapi.test.com/users/

//==Good==//
http://restapi.test.com/users
```

### _(underbar) 대신 -(dash)를 사용한다.
* __-(dash)의 사용도 최소한으로 설계한다.__ 정확한 의미나 표현을 위해 단어의 결합이 불가피한 경우 반드시 -(dash) 사용한다.

```
//==Bad==//
http://restapi.test.com/users/my_comments

//==Good==//
http://restapi.test.com/users/my-comments
```

### 소문자를 사용한다.

```
//==Bad==//
http://restapi.test.com/users/myComments

//==Good==//
http://restapi.test.com/users/my-comments
```

### 행위(method)는 URL에 포함하지 않는다.

```
//==Bad==//
http://restapi.test.com/users/1/list-mypost

//==Good==//
http://restapi.test.com/users/1/posts
```

### 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.
* 함수처럼, 컨트롤 리소스를 나타내는 URL은 동작을 포함하는 이름을 짓는다.

```
//==Bad==//
http://restapi.test.com/users/duplicating

//==Good==//
http://restapi.test.com/users/duplicate
```

## 2. HTTP Headers
### Content-Location
* 요청의 응답 헤더에 새로 생성된 리소스를 식별할 수 있는 __Content-Location 속성__ 을 이용한다.
* HATEOAS로 Content-Location를 대체할 수 있다.

```
HTTP/1.1 200 OK
Content-Location: /users/1
```

### Content-Type
* __application/json__ 사용
* application/xml 등을 제공해서 응답 포맷을 이원화 X, 응답 포맷을 여러 개로 나누면 요청 포맷도 나눠야 한다.

### Retry-After
* 비정상적인 방법(DoS, Brute-force attack)으로 API 서버를 이용하려는 경우 `429 Too Many Requests` 오류 응답과 함께 일정 시간 뒤 요청할 것을 나타낸다.

```
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
```

### Link

> 고유 한 URL을 구성하는 대신 링크 헤더 값을 사용하여 호출을하는 것이 중요합니다.

```
Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next",
  <https://api.github.com/user/repos?page=50&per_page=100>; rel="last"
```

| <center>이름</center> |  <center>기술</center> |
|:--------:|:--------:|
| next | 바로 다음 페이지 결과에 대한 링크 관계|
| last | 결과의 마지막 페이지에 대한 링크 관계|
| first | 결과의 첫 페이지에 대한 링크 관계|
| prev | 바로 이전 결과 페이지에 대한 링크 관계입니다|

[참고: github](https://developer.github.com/v3/#pagination)

## 3. HTTP methods
### POST, GET, PUT, DELETE 4가지 methods는 반드시 제공한다.
### OPTIONS, HEAD, PATCH를 사용하여 완성도 높은 API를 만든다.
* [참고: OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)
* [참고: HEAD](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD)
* [참고: PATCH를](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)

## 4. HTTP status
* [참고: HTTP status](https://sanghaklee.tistory.com/61)

### 의미에 맞는 HTTP status를 리턴한다

```
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
### HTTP status만으로 상태 에러를 나타낸다
* 세부 에러 사항은 응답 객체에 표시하거나, 해당 에러를 확인할 수 있는 link를 표시한다. 
* http 상태 코드를 응답 객체에 중복으로 표시할 필요 없다.

```
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

### 성공 응답은 2XX로 응답한다.
* 200 : [OK]
* 201 : [Created]
* 202 : [Accepted]
* 204 : [No Content]

### 실패 응답은 4XX로 응답한다.
* 400 : [Bad Request]
* 401 : [Unauthorized]
* 403 : [Forbidden]
* 404 : [Not Found]
* 405 : [Method Not Allowed]
* 409 : [Conflict]
* 429 : [Too Many Requests]

### 5XX 에러는 절대 사용자에게 노출하지 마라
* API Server level에선 500 에러가 나선 안된다. (서비스 장애가 발생한 것이기 때문)
* 즉, API Server는 모든 발생 가능한 에러를 핸들링해야 한다.
* 만약 API Server를 서빙하는 웹서버(apache, nginx)가 오류일 때는 500 가능

## 5. HATEOAS
* REST API가 아닌 HTML 환경에선 눈에 보이는 화면이 있기 때문에 __POST /users__ 후에 사용자의 상태가 전이될 수 있는 link를 화면에서 제공할 수 있다.
* 이 문제를 해결하기 위해 응답 객체에 해당 리소스의 상태가 전이될 수 있는 link들을 함께 제공한다. link들을 통해 리소스의 다음 상태 전이 정보를 동적으로 제공한다.

```
{
  "rel": "self",
  "href": "http://restapi.test.com/users/1",
  "method": "GET"
}
```

## 6. Paging
* 어떤 key로 paging을 처리할지 변경될 수 있으니 개발자는 코드의 설정 값으로 언제든 key 이름을 변경할 수 있게 구현한다.

### HTTP Header의 Link 속성을 이용

```
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

### HATEOAS로 응답

```
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

### Link, HATEOAS 모두 사용

```
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

## 7. Ordering
* Collection(리스트)에 대한 GET 요청의 경우(GET /users) 리스트를 클라이언트의 요청에 맞게 정렬해 응답한다.
* __order라는 key를 사용__ 한다.
  + 오름차순: key
  + 내림차순: -key

```
GET /users?order=name

// ?order=-name: name 내림차순 name desc
// ?order=-name,level: name 내림차순, level 오름차순 name desc, level asc
```

## 8. Filtering
* Collection(리스트)에 대한 GET 요청의 경우(GET /users) 리스트 검색 조건을 요청할 수 있다.
  + AND, OR
  + =, !=
  + >, >=
  + <, >=
  + IN(OR), NOT IN
  + LIKE(include)

## 9. Field-Selecting

```
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

## 10. Versioning
* URI Versioning을 채택하고, 버저닝 정보는 host레벨이 아닌 path레벨에 명시한다.
* 예외적으로 서비스의 기본 도메인이 __3차인 경우 path level에 모두 명시한다.__

```
//==Bad==//
http://restapiv1.test.com

//==Good==//
http://restapi.test.com/v1
```

## Reference
* [출처: 이상학의 개발블로그](https://sanghaklee.tistory.com/57)
* [출처: github-pagination](https://developer.github.com/v3/#pagination)