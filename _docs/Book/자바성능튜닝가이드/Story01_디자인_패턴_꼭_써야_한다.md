# Story1. 디자인 패턴, 꼭 써야 한다
생성일: 2022년 6월 11일 오후 5:35

---

## MVC(Model, View, Controller) 모델

JSP, 스윙(Swing) 처럼 화면에 모든 처리(**모델1 방식**) 로직을 모아두는 게 아닌 모델, 뷰, 컨트롤러의 각각의 역할을 담당하는 클래스를 만들어서 개발하는 모델

- `view`: 사용자가 결과를 보거나 입력할 수 있는 화면, 이벤트를 발생시키고 그 결과를 보여주는 역할
- `controller`: **view**와 **model**의 연결자 역할, 뷰에서 받은 이벤트를 모델로 연결하는 역할
- `model`: view에서 입력된 내용을 저장, 관리, 수정하는 역할, 이벤트에 대한 실질적인 일을 수행

## 모델1, 모델2

- **모델1** 방식은 자바 빈을 호출하고 DB에서 정보를 조회, 등록, 수정, 삭제 등의 기능을 수행을 JSP 파일 내부에서 한번에 처리하는 방식이다.
    - 하나의 파일에 MVC 중 뷰, 모델 역할을 한번에 수행(컨트롤러는 존재하지 않음)
- **모델1**은 개발할 때는 빠르고 간단하게 개발할 수 있는 장점이 있지만, 요구사항이 변경되는 등 시간이 갈수록 유지보수가 어렵다는 단점이 있다.
- **모델2** 방식은 모델1의 단점을 해결하기 위해 나온 방식으로 **MVC 모델**이라고 불린다.
- **모델2** 방식은 모든 로직을 JSP 하나의 파일에서 처리하던 모델1과 달리 **서블릿** 이라고 하는 클래스에 요청을 보내서 처리하는 차이점이 있다.
    - 여기서 서블릿은 MVC 중 controller 역할을 수행한다.

## 디자인 패턴

> pattern: 무엇인가를 만들기 위한 모델이나 가이드, 설명의 집합을 의미
> 
- `**디자인 패턴**`은 시스템을 만들기 위해서 전체 중 **일부 의미 있는 클래스들을 묶은 각각의 집합**
- 반복되는 의미 있는 집합을 정의하고 이름을 지정한 것

## J2EE 디자인 패턴

![story1-1](/images/Book/자바성능튜닝가이드/story1-1.png)


✅: 성능 관련 및 알아두면 좋은 패턴

- `Intercepting Filter 패턴`: 요청 타입에 따라 다른 처리를 하기 위한 패턴
- `Front Controller 패턴`: 요청 전후에 처리하기 위한 컨트롤러를 지정하는 패턴
- `View Helper 패턴`: 프레젠테이션 로직과 상관 없는 비즈니스 로직을 핼퍼로 지정하는 패턴
- `Composite View 패턴`: 최소 단위의 하위 컴포넌트를 분리하여 화면을 구성하는 패턴
- `Service to Worker 패턴`: Front Controller와 View Helper 사이에 디스패처를 두어 조합하는 패턴
- `Dispatcher View 패턴`: Front Controller와 View Helper로 디스패처 컴포넌트를 형상한다. 뷰 처리가 종료될 때까지 다른 활동은 지연한다는 점이 Service To Worker 패턴과 다르다.
- ✅`Business Delegate 패턴:` 비즈니스 서비스 접근을 캡슐화하는 패턴
- ✅`Service Locator 패턴`: 서비스와 컴포넌트 검색을 쉽게하는 패턴
- ✅`Session Facade 패턴`: 비즈니스 티어 컴포넌트를 캡슐화하고, 원격 클라이언트에서 접근할 수 있는 서비스를 제공하는 패턴
- `Composite Entity 패턴`: 로컬 엔티티 빈과 POJO를 이용하여 큰 단위의 엔티티 객체를 구현
- ✅`Transfer Object 패턴`: 일명 VO(Value Object) 패턴이라고 많이 알려진 패턴, 데이터를 전송하기 위한 객체에 대한 패턴
- `Transfer Object Assembler 패턴`: 하나의 Transfer Object로 모든 타입 데이터를 처리할 수 없으므로, 여러 Transfer Object를 조합하거나 변형한 객체를 생성하여 사용하는 패턴
- `Value List Handler 패턴`: 데이터 조회를 처리하고, 결과를 임시 저장하며, 결과 집합을 검색하여 필요한 항목을 선택하는 역할을 수행
- ✅`Data Access Object 패턴`: 일명 DAO라고 많이 알려진 패턴, DB에 접근을 전담하는 클래스를 추상화하고 캡슐화
- `Service Activator 패턴`: 비동기적 호출을 처리하기 위한 패턴

### Transfer Object 패턴

VO(Value Object)라고 불리는 **Transfer Object** 패턴은 **데이터를 전송하기 위한 객체**에 대한 패턴이다. **객체에 여러 타입의 값을 전달**하는 일을 수행한다.

```java
@Getter @Setter
public class MemberRequestTO implements Serializable {
		private Long id;
		private String name;
		private int age;

		public MemberRequestTO(Long id, String name, int age) {
			...
		}

		public String getName() {
				if (name == null) {
					return "";
				}
				return name;
		}
		
		public String toString() {
				...
		}
}
```

**Transfer Object** 패턴을 사용할 때, 고려할 수 있는 방법이 있다.

1. 객체 인스턴스의 **접근 제어자를 private로 지정**하고 getter, setter 메소드를 생성하여 인스턴스에 접근하는 방법
2. 객체 인스턴스의 **접근 제어자를 public로 지정**하고 직접 접근하여 메소드를 만들지 않는 방법

**성능상으로 따져 볼 때,** getter, setter 메소드를 만들지 않는 것(**2번 방식**)이 더 빠르다. 

다만, 정보를 은닉하고, 모든 필드의 값들을 아무나 수정할 수 없게 하려면 getter, setter 메소드를 정의하는 것이 좋다. 또한, getName() 메소드 처럼 name 값이 null이면 null 값을 리턴하는 것이 아니라 “” 빈 값을 리턴하도록 할 수 있다. 

즉, Transfer Object를 잘 만들어 놓으면 각 필드가 null 체크를 할 필요가 없어진다.

추가로, **Transfer Object를** 생성할 때, 반드시 toString 메소드를 구현하는 것이 좋다. 만약 해당 메소드를 구현하지 않고 toString() 을 호출할 경우 MemberRequestTO@2140 처럼 알 수 없는 값을 리턴한다.

> Serializable 구현, Serializable를 구현하면 해당 객체를 **직렬화**할 수 있다. 다시말해, **서버 사이의 데이터 전송이 가능**해진다. 그러므로 원격지 서버에 데이터를 전송하거나, 파일로 객체를 저장할 경우에는 Serializable 인터페이스를 구현해야한다.
> 

**Transfer Object**를 사용한다고 엄청난 성능 개선 효과가 발생하는 것은 아니다.

다만, **하나의 객체에 여러 결과 값을 담아**서 요청, 결과를 받을 수 있어 객체 인스턴스 개수 만큼 여러번 요청하는 일이 발생하는 것을 줄여준다.

### [참고] Serializable ??

여기서 드는 **의문점,** 스프링에서 @ResponseBody, @RequestBody에 매핑하기 위해 전송용 객체 DTO, Model를 구현하곤 하는데 이때 해당 객체들은 직렬화되는 게 아닌가?? Serializable 인터페이스를 구현하지 않았는데 어떻게 직렬화돼서 전송할 수 있는걸까??

직렬화란 정확히 무엇인가?? 파일 전송, 소켓의 경우에 사용되는 것일까??

결론부터 말하면 DTO, Model 객체들도 직렬화된 것이 맞다. 

그러나 정확히 말하면 **Java의 Serializable로 처리되는 것은 아니고**, @ResponseBody, @RequestBody로 매핑된 객체들은 **JSON으로 직렬화**된 것이다. 

이 때 **HttpMessageConverter에 의해** Object  → JSON, JSON → Object로 직렬화가 수행되는 것이다.

- Json Serialized And Java Serializable: [https://stackoverflow.com/questions/26948289/is-it-necessary-to-serialize-objects-for-using-requestbody-responsebody-annota](https://stackoverflow.com/questions/26948289/is-it-necessary-to-serialize-objects-for-using-requestbody-responsebody-annota)

그렇다면, **Java의 Serializable**는 무엇인가? 

직렬화란, 객체를 다른 서버(**IP가 다른**)로 보낼 때, byte stream 으로 변환하여 전송 가능한 형태로 만드는 것

- Java Serializable: [https://devlog-wjdrbs96.tistory.com/268](https://devlog-wjdrbs96.tistory.com/268)

> **Serializable 인터페이스**를 구현할 때, `private static final long serialVersionUID = 1L` 값을 지정해 주는데 해당 값을 지정하지 않으면 컴파일 시점에 자동으로 생성된다.
그리고, 반드시 `static final long serialVersionUID`으로 선언해야 자바에서 인식할 수 있다.
> 

JPA와 Serializable

> JPA 표준 스펙은 Entity에 Serializable를 구현하도록 되어 있습니다. JPA 구현체에 따라 엔티티를 분산 환경에서 사용할 수 있거나, 직열화해서 다른 곳에 전송할 수 있는 가능성을 열어준거지요.
> 
> - JPA Entity Serializable1: [https://www.inflearn.com/questions/17117](https://www.inflearn.com/questions/17117)
> - JPA Entity Serializable2: [https://www.inflearn.com/questions/16570](https://www.inflearn.com/questions/16570)

### Service Locator 패턴

**Service Locator**는 **의존성 문제**를 해결하기 위한 방법으로 **Service Locator와 DI**가 사용된다고 한다.(이 둘은 **IoC** 구현 방법 중 하나)

**Service Locator**는 마틴 파울러에 의해 유명해진 디자인 패턴인데(안티패턴이라고 하는 내용이 있음), 예전에 많이 사용되던 **EJB의 EJB Home 객체나 DB의 DataSource를 찾을 때(Lookup) 소요되는 응답 속도를 감소시키기 위해서 사용**된다.

```java
public class ServiceLocator {
		private InitialContext ic;
		private Map cache;
		private static ServiceLocator me;
		static {
				me = new ServiceLocator();
		}
		private SerivceLocator() {
				cache = Collections.synchronizedMap(new HashMap());
		}
	
		public InitialContext getInitialContext() throws Exception {
				try {
						if( ic == null) {
								ic = new InitialContext();
						}
				} catch (Exception e) {
						throw e;
				}
				return ic;
		}
	
		public static ServiceLocator getInstance(){
				return me;
		}
	
		public EJBLocalHome getLocalHome(String jndiHomeName) throws Exception {
				EJBLocalHome home = null;
				try {
						if (cache.containsKey(jndiHomeName)) {
								home = (EJBLocalHome)cache.get(jndiHomeName);
						} else {
								home = (EJBLocalHome)getInitialContext().lookup(jndiHomeName);
								cache.put(jndiHomeName, home);
						}
				} catch (NamingException ne) {
						throw new Exception(ne.getMessage());
				} catch (Exception e) {
						throw new Exception(e.getMessage());
				}
				return home;
		}
};
```

코드를 살펴보면, cache라는 Map 객체에 home 객체를 찾은 결과를 보관하고 있다가, 누군가 그 객체를 필요로 할 때 메모리에서 찾아서 제공하도록 되어있다. 만약 해당 객체가 cache라는 Map에 없으면 메모리에서 찾는다.

### [참고] Service Locator는 안티패턴?

**생성자 주입(**Dependency Injection**)**을 사용한다면 컴파일러는 코드를 소비하는 사람과 생산하는 사람 모두에게 도움이 된다. Service Locator에 의존하는 API라면 이런 도움을 전혀 받을 수 없다.

- Service Locator 는 [SOLID 원칙을 위반한다는 점](http://blog.ploeh.dk/2014/05/15/service-locator-violates-solid/)에서 부정적이다.
    - 인터페이스 분리 원칙(ISP) 위반
- Service Locator 의 근본적인 문제는 [캡슐화를 위반해서](http://blog.ploeh.dk/2015/10/26/service-locator-violates-encapsulation/) 나타난다.

- Service Locator는 안티패턴(번역): [https://edykim.com/ko/post/the-service-locator-is-an-antipattern/](https://edykim.com/ko/post/the-service-locator-is-an-antipattern/)
- Service Locator:  [https://incheol-jung.gitbook.io/docs/study/undefined/di](https://incheol-jung.gitbook.io/docs/study/undefined/di)
- Service Locator와 DI 차이점: [https://code.i-harness.com/ko-kr/q/17c515](https://code.i-harness.com/ko-kr/q/17c515)

# References