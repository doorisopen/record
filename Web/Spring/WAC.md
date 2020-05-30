
> WAC를 알기 전에 Application Context와 BeanFactory에 대해서 알아보자

## BeanFactory
* __빈의 생성, 빈의 의존관계 관리등의 DI의 기본 기능을 제공__
* 빈이 많지 않고 경량 컨테이너로 작업할 때 활용
* XML 파일로부터 설정 정보를 활용하는 가장 많이 사용되는 클래스
* org.springframework.beans.factory.xml.XmlBeanFactory

![web-spring-wac_beanfactory](/images/Web/Spring/web-spring-wac_beanfactory.JPG)

## Application Context
* 일반적인 스프링 컨테이너를 의미
* BeanFactory 인터페이스를 상속받은 하위 인터페이스로 확장된 기능 제공
  + Internationalization, AOP, Transaction Management
* XML 파일로부터 설정 정보를 활용하는 가장 많이 사용되는 클래스
* org.springframework.context.support.ClassPathXmlApplicationContext

![web-spring-wac_applicationcontext](/images/Web/Spring/web-spring-wac_applicationcontext.JPG)

## WAC(Web Application Context)란?
* __웹 애플리케이션을 위한 ApplicationContext__
* org.springframework.web.context.support.XmlWebApplicationContext

## WAC의 종류
* __ContextLoaderListener에 의해 생성되는 WAC__
  + Persistence(DAO), Service 관련 스프링 빈들을 등록
  + 웹 어플리케이션 전체에서 사용할 WAC 객체 생성
  + root-context.xml 파일에 설정
* __DispatcherServlet에 의해 생성되는 WAC__
  + 컨트롤러와 같은 서블릿 관련 빈 등록
  + 해당 서블릿 마다 사용할 WAC 객체 생성
  + servlet-context.xml 파일에 설정

## 웹 어플리케이션에서 컨테이너 인스턴스화
* web.xml에서 __ConetxtLoadListener, DispatcherServlet 를 사용하여 ApplicationContext 생성__
![web-spring-wac_applicationcontext_webxml](/images/Web/Spring/web-spring-wac_applicationcontext_webxml.JPG)

## 웹 어플리케이션 컨텍스트의 2가지 생성 과정
1. 웹 어플리케이션이 요청되면 WAS에 의해 web.xml이 로드 된다.
2. web.xml에 등록한 contextLoadListener가 생성되고 생성된 contextLoadListener는 root-context.xml을 로드한다.
3. root-context.xml에 등록된 spring Container가 구동 되고, 이때 개발자가 작성한 비즈니스 로직 DAO, VO 객체를 생성한다.
4. 클라이언트로부터 웹 어플리케이션이 요청이 오면 DispatcherServlet이 생성되고 servlet-context.xml을 로드한다.
이는 FrontController의 역할을 수행한다. 클라이언트로부터 요청 온 메시지를 분석하여 알맞은 pageController에게 전달하고 응답받아 요청에 따른 응답을 어떻게 할지 결정만 한다.

# Question
## 웹 어플리케이션 컨텍스트의 2가지 생성 과정을 설명해주세요

# References