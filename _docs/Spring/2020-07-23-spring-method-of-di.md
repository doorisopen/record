---
title: 의존관계 주입(DI, Dependency Injection) 방법
category: Spring
date:   2020-07-23 00:30:59
comments: true
order: 5
---

Spring Framework에서 중요한 특징 중 하나인 __의존관계 주입(DI, Dependency Injection)__ 방법에 대해서 알아보겟습니다.

## 목차
* 의존성 주입(DI, Dependency Injection)란?
  + 애플리케이션의 의존관계 형태
    - 일반적인 애플리케이션 : new 연산자 활용
    - DI 컨테이너 활용 : new 연산자 제거
    - DI 컨테이너 활용 : Interface
* 의존성 주입 방법
  + XML 기반 설정
  + Annotation 기반 설정
  + Java 기반 설정

## DI(Dependency Injection) 란?
* __의존 관계 주입__ 을 의미한다.
* __객체 간의 의존 관계__ 를 만드는 것이다.
* Spring의 __IOC 컨테이너의 구체적인 구현 방식이다.__
* 스프링 프레임워크는 __런타임시 사용할 객체들의 의존 관계를 부여__ 한다.
* 객체 간의 __결합도를 낮춘다.__

의존성 주입 방법에 대해서 알아보기 전에 __애플리케이션의 의존관계 형태__ 를 알아보겠습니다.

#### new 연산자 활용
일반적인 애플리케이션에서 __new 연산자__ 를 사용해서 의존 관리를 했습니다. 

다음은 Main이 MemberService 객체를 new로 생성, MemberService가 MemberDAO객체를 생성하고 해당 인스턴스를 이용하는 형태와 코드입니다.

![web-spring-di-use-new]({{ site.baseurl }}{{ site.spring_img }}/web-spring-di-use-new.JPG)

```java
Class MemberService {
    MemberDAO memberDAO = new MemberDAO();
    
    public int save(MemberVO member) {
        return memberDAO.save(member);
    }
    ...
}
```

#### DI Container 활용 : new 연산자 제거
일반적인 애플리케이션과 달리 스프링에서는 의존성 주입(DI)를 개발자가 직접(new 연산자 활용) 하지 않고 DI 컨테이너가 대신 해줍니다. 이러한 개념은 스프링에서 __"인스턴스를 제어하는 주도권이 역전 되었다" 라는 의미의 IOC(Inversion of Control)__ 라고 합니다. 

DI와 IOC에 대한 내용은 [DI와 IoC](https://doorisopen.github.io/developers-library/Spring/2020-06-04-spring-di-and-ioc/)를 참고해주세요.

![web-spring-di-use-container-delete-new]({{ site.baseurl }}{{ site.spring_img }}/web-spring-di-use-container-delete-new.JPG)

위의 형태를 보면 new 연산자를 사용하지 않습니다. 

그리고 Main이 이용하는 Service와 Service가 이용하는 DAO 인스턴스를 __DI 컨테이너가 대신 생성__ 해주고 있음을 볼 수 있습니다. 또한, DAO 인스턴스를 Service에 의존관계 주입(DI)하고 있습니다.

> 개발자의 어떤 설정으로 DI 컨테이너가 각각의 인스턴스들의 의존관계를 만드는지에 대해서는 __의존성 주입 방법__ 에서 설명하겠습니다.

#### DI Container 활용 : Interface
인터페이스 기반의 컴포넌트화를 하려면 MemberService와 MemberDAO를 인터페이스로 만들고 구현 클래스는 Impl을 덧붙입니다(MemberServiceImpl).

![web-spring-di-use-container-interface]({{ site.baseurl }}{{ site.spring_img }}/web-spring-di-use-container-interface.JPG)

> ※참고※ <br/>
> __인터페이스 기반 컴포넌트화__ 하는 이유? <br/>
> 소프트웨어 공학에서 결합도, 응집도라는 개념이 있습니다. 이때 결합도는 낮추고 응집도를 높여야 좋은 소프트웨어라고 합니다. 결합도를 낮추기 위한 방법으로 비즈니스 로직에 인터페이스를 두는 방법과 DI을 통해 결합도를 낮출 수 있습니다.

## 의존성 주입 방법
XML, Annotation, Java 기반 설정을 통해서 객체간의 의존 관계를 설정합니다.

#### XML
* __생성자 기반 의존성 주입(Constructor based dependency Injection)__
  + 생성자의 인수를 사용해 의존성을 주입
  + 설정파일 XML에 <constructor-arg> 태그를 사용하여 주입할 컴포넌트를 설정
* __설정자 기반 의존성 주입(Setter based dependency Injection)__
  + 메서드의 인수를 통해 의존성을 주입
  + 설정파일 XML에 <property> 요소의 name 속성에 주입할 컴포넌트의 이름을 설정

##### 생성자 기반 의존성 주입
__생성자 기반__ 의존성 주입방법에 대해서 알아보겠습니다.

```xml
<!-- applicationContext.xml -->
<bean id="memberDAO" class="com.test.di.persistence.MemberDAOImpl"></bean>
<bean id="memberService" class="com.test.di.service.MemberServiceImpl">
    <constructor-arg ref="memberDAO"/>
</bean>
```

xml 파일에 service와 dao 인스턴스들을 빈으로 등록합니다. 그리고 <constructor-arg> 태그를 사용해서 service와 dao의 의존관계를 설정해 줍니다.

```java
// MemberServiceImpl.java
public class MemberServiceImpl implements MemberService {
    
    private MemberDAO memberDAO;

    public MemberServiceImple( MemberDAO memberDAO ) {
        this.memberDAO = memberDAO;
    }
    ...(생략)
}
```

이렇게 xml에 의존성 주입을 설정이 끝나면 MemberServiceImpl.java 에서 생성자로 dao와의 의존관계를 주입해줍니다. 

##### 설정자(Setter) 기반 의존성 주입
__설정자(Setter) 기반__ 의존성 주입 방법을 알아보겠습니다.

```xml
<!-- applicationContext.xml -->
<bean id="memberDAO" class="com.test.di.persistence.MemberDAOImpl"></bean>
<bean id="memberService" class="com.test.di.service.MemberServiceImpl">
    <property name = "memberDAO" ref="memberDAO" />
</bean>
```

생성자 방식과 다르게 <constructor-arg> 태그가 아닌 <property> 태그를 사용해서 의존관계를 설정해 줍니다.

```java
// MemberServiceImpl.java
public class MemberServiceImpl implements MemberService {
    
    private MemberDAO memberDAO;

    public setMemberDAO( MemberDAO memberDAO ) {
        this.memberDAO = memberDAO;
    }
    ...(생략)
}
```

MemberServiceImpl.java 에는 생성자가 아닌 setXXX()로 의존관계를 주입해줍니다.

#### Annotation
* xml에 빈을 미리 등록하고 @Autowired로 의존성 주입하기
  + __@Autowired는 컨테이너가 빈과 다른 빈과의 의존성을 자동으로 연결하도록 하는 수단입니다.__
  + xml에 <context:annotation-config/> 설정
* @Component와 @Autowired로 의존성 주입하기
  + __@Component는 컨테이너가 인젝션을 위한 인스턴스(빈)을 설정하는 수단입니다.__
  + 등록된 인스턴스(빈)에 @Autowired를 사용하여 의존관계를 주입해준다.
  + xml에 <context:component-scan base-package=""/> 설정

##### @Autowired로 의존성 주입하기
먼저 @Autowired와 xml에 <context:annotation-config/> 설정으로 의존성 주입하는 방법을 알아보겠습니다.

```xml
<!-- applicationContext.xml -->
<bean id="memberDAO" class="com.test.di.persistence.MemberDAOImpl"></bean>
<bean id="memberService" class="com.test.di.service.MemberServiceImpl"></bean>
<context:annotation-config /> 
```

xml에는 __service와 dao의 빈들을 등록__ 해 줍니다. 그리고 <context:annotation-config /> 태그를 등록해줍니다. 

* <context:annotation-config />
  + @Autowired, @Resource, @Required를 이용할 때 선언
  + __XML 파일에 이미 등록된 빈들의 애노테이션 기능을 적용하기 위해 선언__
  + __빈을 등록하기 위한 검색 기능은 없다.__

```java
// MemberServiceImpl.java
...
import org.springframework.beans.factory.annotation.Autowired;

public class MemberServiceImpl implements MemberService {
    
    @Autowired // 의존관계 주입
    private MemberDAO memberDAO;
    
    ...(생략)
```

xml설정이 끝났다면 빈과 다른 빈과의 의존성을 자동으로 연결해주는 @Autowired를 사용하면 의존관계가 설정이 완료 됩니다.

##### @Component와 @Autowired로 의존성 주입
다음은 @Component와 @Autowired로 의존성 주입하는 방법을 알아보겠습니다.

```xml
<!-- applicationContext.xml -->
<context:component-scan base-package="com.test.di.persistence"></context:component-scan>
<context:component-scan base-package="com.test.di.service"></context:component-scan>
```

xml에 위와 같이 <context:component-scan> 태그를 입력해 줍니다. 

* <context:component-scan base-package="패키지명"/>
  + @Component, @Service, @Repository, @Controller등의 컴포넌트 이용할 때 선언
  + __특정 패키지 안에 클래스를 검색해서 빈을 자동으로 찾아서 DI 컨테이너에 등록.__
  + 이 설정을 사용할 경우에는 <context:annotation-config /> 를 선언할 필요가 없다.

```java
// MemberDAOImpl.java
@Component // 빈 등록
public class MemberDAOImpl implements MemberDAO {
    ...
}

// MemberDAOImpl.java
@Component // 빈 등록
public class MemberServiceImpl implements MemberService {

    @Autowired // 의존관계 주입
    private MemberDAO memberDAO;
    
    ...
}
```

그리고 위와 같이 빈으로 등록할 클래스들을 @Component을 선언해 줍니다. 이렇게 되면은 xml에 설정한 <context:component-scan> 의 base-package를 검색해서 @Component가 설정된 클래스들을 찾아서 빈으로 등록하게됩니다.

그리고 @Autowired를 사용하여 의존관계를 주입하게 됩니다.

#### Java
Java를 이용한 의존성 주입 설정은 __XML 문법 대신 자바 코드로 빈을 설정__ 합니다.

* Java로 DI 설정 코드의 장점은 TypeSafe하고 Refactoring에 매우 적합합니다.
* 프로퍼티 명 또는 클래스 명이 틀렸을 경우 컴파일 에러를 냅니다.
* Spring3.0 버전부터 애노테이션을 이용하여 빈과 관련된 정보를 설정할 수 있게 됩니다.
* XML 설정 없이 자바 코드를 이용해서 빈 객체 생성과 빈 객체간의 의존 관계 설정할 수 있습니다.
* 하나의 클래스 안에 복수의 빈을 등록할 수 있습니다.

```java
// JavaConfig.java
@Configuration
public class JavaConfig {
    @Bean
    public MemberDAO memberDAO() {
        return new MemberDAOImpl();
    }

    @Bean(name="service")
    public MemberService memberService() {
        return new MemberServiceImpl(memberDAO());
    }
}
```

위와 같이 Java를 이용하여 의존성 주입 설정을 할 수 있습니다.

* @Configuration
  + 빈 설정 메타 정보를 담고 있는 클래스를 선언
  + 해당 클래스가 스프링 설정으로 사용됨

* @Bean
  + 클래스 내의 매서드를 정의하여 새로운 빈 객체를 정의할 때 사용
  + name 속성을 사용하여 새로운 빈 이름 적용 가능

```java
public class Main {

    private static ApplicationContext ctx = null;

    public static void main(String[] args) throws Exception {
        ctx = new AnnotationConfigApplicationContext(JavaConfig.class); // 자바설정 클래스
        MemberService memberService = ctx.getBean("service", MemberService.class); // 가져올 빈 이름
    }
}
```

그리고 위와 같이 Main 클래스에서 해당 JavaConfig클래스를 사용할 수 있습니다.

## References
* 모교 웹프레임워크 강의