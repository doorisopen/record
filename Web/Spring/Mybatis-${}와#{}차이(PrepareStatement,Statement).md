## #{}
* #{}을 사용하면, mybatis 는 __PreparedStatement__ 를 생성하고 PreparedStatement 매개변수에 대해서 값을 안전하게 설정한다.
* PreparedStatement 가 제공하는 set 계열의 메소드를 사용하여 물음표(?)를 대체할 값을 지정한다. 
* #{}의 방법은 안전하고 빠르기 때문에 선호된다. 그리고 들어오는 데이터를 문자열로 취급하기 때문에 자동으로 큰 따옴표가 붙는다.

```
//==Example==//
select * from member where username = ?
```

## ${}
* ${}을 사용하면, mybatis 는 __Statement__ 를 생성하고 ${}을 사용하면 해당 인자 값을 그대로 받는다. 따라서 상수 그대로 값을 받기 때문에 해당 컬럼이 문자열인 경우에는 따옴표로 감싸주는 구문을 추가해주어야 한다.
* ${}을 사용하게 되면 SQL Injection 같은 문제에 취약하다. 왜냐하면 문자열을 직접 삽입하기 때문이다.

```
//==Example==//
select * from member where username = 홍길동
```