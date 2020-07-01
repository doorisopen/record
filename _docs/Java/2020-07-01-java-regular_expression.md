---
title: "정규 표현식(Regular Expression)"
category: Java
date: 2020-07-01 00:30:59
comments: true
order: 8
---


## 정규 표현식이란?
* 정규 표현식은 줄여서 정규식이라고도 하며, 영어로는 Regular Expression, 줄여서 regex, regexp라고도 합니다. 
* 정규 표현식은 특정한 규칙을 가진 문자열의 집합을 표현하기 위해 쓰이는 형식언어입니다.

#### 종류
* UNIX 계열: POSIX의 정규 표현식
* POSIX에서 확장: Perl의 정규 표현식

> Java의 경우 Perl와 유사한 방식을 선택하고 있다. 완전히 동일한 것은 아니다. 차이점은 [Oracle문서](https://docs.oracle.com/javase/tutorial/essential/regex/index.html)에서 확인 할 수 있습니다.

## 언제 필요한가?
* 어떤 문자열에서 특정한 조건의 문자열을 찾고 싶을 때, 그 조건이 복잡하다면 정규 표현식이 도움이 될 수 있습니다.
* 예를 들어 회원가입시 아이디와 비밀번호 설정시 조건이 맞춰졌는지 확인할때 사용됩니다.

{% highlight javascript %}
//==정규 표현식 예시==//
String pwPattern = "^(?=.*[!@#$])(?=.*[a-zA-Z])(?=.*[0-9]).{8,20}$";
{% endhighlight %}

## 정규 표현식 문법

![java-regex_grammar]({{ site.baseurl }}/images/Language/Java/java-regex_grammar.JPG)

## 정규 표현식 사용하기
자바에서 정규표현식을 사용할때에는 `java.util.regex` 패키지 안에 있는 __Pattern클래스__ 와 __Matcher클래스__ 를 주로 사용합니다.

## Pattern 클래스
* Pattern 클래스는 __정규 표현식에 대상 문자열 검증__ 을 java.util.regex.Pattern 클래스의 __matches()__ 를 사용하여 검증할 수 있습니다.
* matches()의 __첫 번째 매개값은 정규표현식__ 이고 __두 번째 매개값은 검증 대상 문자열__ 이다. 일치하면 true, 불일치 false 반환합니다.

{% highlight javascript %}
String pattern = "^[0-9]*$"; // 정규표현식 
String str = "12345678"; // 검증 대상

boolean isMatch = Pattern.matches(pattern, str); // isMatch: true
{% endhighlight %}

#### Pattern 클래스 주요 메서드
* __compile(String regex) :__ 주어진 정규표현식으로부터 패턴을 만듭니다.
* __matcher(CharSequence input) :__ 대상 문자열이 패턴과 일치할 경우 true를 반환합니다.
* __asPredicate() :__ 문자열을 일치시키는 데 사용할 수있는 술어를 작성합니다.
* __pattern() :__ 컴파일된 정규표현식을 String 형태로 반환합니다.
* __split(CharSequence input) :__ 문자열을 주어진 인자값 CharSequence 패턴에 따라 분리합니다.

#### Parttern 플래그 값 사용(상수)
* __Pattern.CANON_EQ :__ None표준화된 매칭 모드를 활성화합니다.
* __Pattern.CASE_INSENSITIVE :__ 대소문자를 구분하지 않습니다. 
* __Pattern.COMMENTS :__ 공백과 #으로 시작하는 주석이 무시됩니다. (라인의 끝까지).
* __Pattern.MULTILINE :__ 수식 '^' 는 라인의 시작과, '$' 는 라인의 끝과 match 됩니다.
* __Pattern.DOTALL :__ 수식 '.'과 모든 문자와 match 되고 '\n' 도 match 에 포함됩니다.
* __Pattern.UNICODE_CASE :__ 유니코드를 기준으로 대소문자 구분 없이 match 시킵니다.
* __Pattert.UNIX_LINES :__ 수식 '.' 과 '^' 및 '$'의 match시에 한 라인의 끝을 의미하는 '\n'만 인식됩니다.

## Matcher 클래스
* Matcher 클래스는 __대상 문자열의 패턴을 해석__ , 주어진 __패턴과 일치하는지 판별__ 해줍니다.
* Matcher 클래스의 입력값으로는 CharSequence라는 새로운 인터페이스가 사용되는데 이를 통해 다양한 형태의 입력 데이터로부터 문자 단위의 매칭 기능을 지원 받을 수 있습니다.
* Matcher객체는 Pattern객체의 matcher() 메소드를 호출하여 받아올 수 있습니다.

{% highlight javascript %}
Pattern pattern = Pattern.compile("^[a-zA-Z]*$"); // 영문(소,대문자)만
String str1 = "12345678"; // 검증 대상1
String str2 = "abcdEFg"; // 검증 대상2

Matcher isMatch1 = pattern.matcher(str1); 
Matcher isMatch2 = pattern.matcher(str2);

System.out.println(isMatch1.find()); // false
System.out.println(isMatch2.find()); // true
{% endhighlight %}

#### Matcher 클래스 주요 메서드
* __matches() :__ 대상 문자열과 패턴이 일치할 경우 true 반환합니다.
* __find() :__ 대상 문자열과 패턴이 일치하는 경우 true를 반환하고, 그 위치로 이동합니다.
* __find(int start) :__ start위치 이후부터 매칭검색을 수행합니다.
* __start() :__ 매칭되는 문자열 시작위치 반환합니다.
* __start(int group) :__ 지정된 그룹이 매칭되는 시작위치 반환합니다.
* __end() :__ 매칭되는  문자열 끝 다음 문자위치 반환합니다.
* __end(int group) :__ 지정되 그룹이 매칭되는 끝 다음 문자위치 반환합니다.
* __group() :__ 매칭된 부분을 반환합니다.
* __group(int group) :__ 매칭된 부분중 group번 그룹핑 매칭부분 반환합니다. 
* __groupCount() :__ 패턴내 그룹핑한(괄호지정) 전체 갯수를 반환합니다.


## 정규 표현식 연습문제
* ex1, 숫자(0-9)
* ex2, 영문(소,대문자)
* ex3, 한글
* ex4, 영문(소,대문자) + 숫자 + (영문소, 영문대, 숫자 최소 1글자 이상)
* ex5, 영문(소,대문자) + 숫자 + 5~10자
* ex6, 영문(소,대문자) + 숫자 + 8~20자 + (영문, 숫자 최소 1글자 이상)
* ex7, 영문(소,대문자) + 숫자 + 특수문자 + 8~20자 + (영문, 숫자, 특수문자 최소 1글자 이상)
* ex8, 영문(소,대문자) + 숫자 + 특수문자 + 공백문자 허용X + 8~20자 + (영문, 숫자, 특수문자 최소 1글자 이상)
* ex9, E-mail(ex, hello@gmail.com)
* ex10, 전화번호(ex, 010-1234-5678)
* ex11, 주민등록번호(ex, 800131-1234565)
* ex12, 같은 문자 4개 이상 사용 불가

{% highlight javascript %}
//== ex1, 숫자(0-9) ==//
String pattern = "^[0-9]*$";

//== ex2, 영문(소,대문자) ==//
String pattern = "^[a-zA-Z]*$";

//== ex3, 한글 ==//
String pattern = "^[가-힣]*$";

//== ex4, 영문(소,대문자) + 숫자 + (영문소, 영문대, 숫자 최소 1글자 이상) ==//
String pattern = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])$";

//== ex5, 영문(소,대문자) + 숫자 + 5~10자 ==//
String pattern = "^[a-z0-9].{5,10}$";

//== ex6, 영문(소,대문자) + 숫자 + 8~20자 + (영문(소,대문자), 숫자 최소 1글자 이상) ==//
String pattern = "^(?=.*[0-9])(?=.*[a-zA-Z]).{8,20}$";

//== ex7, 영문(소,대문자) + 숫자 + 특수문자 + 8~20자 + (영문소, 영문대, 숫자, 특수문자 최소 1글자 이상) ==//
String pattern = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*\\W).{8,20}";

//== ex8, ex7 + 공백문자 허용X ==//
String pattern = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*\\W)(?=\\S+$).{8,20}";

//== ex9, E-mail(ex, hello@gmail.com) ==//
String pattern = "\\w+@\\w+\\.\\w+(\\.\\w+)?";

//== ex10, 전화번호(ex, 010-1234-5678) ==//
String pattern = "^\d{2,3}-\d{3,4}-\d{4}$";

//== ex11, 주민등록번호(ex, 800131-1234565) ==//
String pattern = "\d{6} \- [1-4]\d{6}";

//== ex12, 같은 문자 4개 이상 사용 불가==//
String pattern = "(\\w)\\1\\1\\1";

{% endhighlight %}

## References
* [https://coding-factory.tistory.com/529](https://coding-factory.tistory.com/529)
* [https://medium.com/@cbs5295](https://medium.com/@cbs5295/java-%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D-regular-expression-%EC%9D%98-%EC%9D%B4%ED%95%B4-31419561e4eb)
* [https://jamesdreaming.tistory.com/195](https://jamesdreaming.tistory.com/195)