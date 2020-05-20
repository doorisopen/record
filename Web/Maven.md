# Maven 이란?
* __프로젝트를 관리하는 도구__
* __빌드 자동화__ 기능과 __프로젝트 관리__ 기능을 제공
  + __프로젝트(라이브러리) 관리__
    - pom.xml 파일을 이용하여 프로젝트 관련된 jar 파일을 다운로드하고 관리
    - 프로젝트 산출물을 일관된 구조로 관리
  + __빌드 자동화__
    - 빌드 작업들을 간단하고 쉽게 그리고 일관성 있게 수행할 수 있는 통합 환경을 제공
    - 빌드는 __소스 코드 파일을 실행 코드로 변환하여 배포하는 과정__


## 프로젝트(라이브러리) 관리
* 프로젝트 관리 설정들을 메이븐이 미리 정의한 설정들로 대체한다.

### 기능1: 정형화된 프로젝트 디렉토리 구조 관리
* Convention over Configuration(CoC) 패러다임을 따름 : 설정보다는 규범
* __웹 디렉토리 구성__
  + java/src와 java/resource 디렉토리는 Maven 기본 디렉토리를 유지하고 __웹 자원을 관리하는 별도의 src/main/webapp 디렉토리를 사용__ 한다.
  + 이 값은 maven-war-plugin의 warSourceDirectory에 설정되어 있음.

![web_maven_1](/images/Web/web_maven_1.JPG)


### 기능2: 의존성 관리기능(pom.xml): 편리한 라이브러리 관리 기능
* 프로젝트 빌드에 필요한 라이브러리, 플러그인을 개발자 PC에 자동으로 다운로드
* __POM(Project Object Model).xml__

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	
    <modelVersion>4.0.0</modelVersion>
	<groupId>com.doop</groupId>
	<artifactId>doop</artifactId>
	<name>doop</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>

	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>4.3.14.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>

	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
        <!-- junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

* __참고: 모든 POM은 Super POM(기본 POM)으로부터 상속받는다.__
  + Super POM은 기본 설정 정보를 포함한다.
  + Super POM을 수정하려면 POM에서 오버라이드한다.
* __참고: Pom.xml scope__
![web_maven_2](/images/Web/web_maven_2.JPG)

* __Maven Repositorty__
  + 중앙 저장소
    - (https://repo.maven.apache.org/maven2), (http://search.maven.org/), (http://mvnrepository.com/)
  + 로컬 저장소
    - (USER_HOME\\.m2\repository)

### 기능3: 빌드 프로세스를 관리(pom.xml)
* 플러그인 설정을 통해 빌드 자동화


### Maven 의존성 검색 절차
* 1단계 : 지역 저장소를 검색한다. 찾는 라이브러리가 없을 경우 2단계로 넘어간다.
* 2단계 : 중앙 저장소를 검색한다. 찾은 라이브러리는 지역 저장소에 저장한다. 만약 찾는 라이브러리가 없을 경우 3단계로 넘어간다. 만약 원격 저장소가 존재하지 않을 경우 에러를 발생시키고 종료한다.
* 3단계 : 원격 저장소를 검색한다. 찾은 라이브러리는 지역 저장소에 저장한다. 만약 찾는 라이브러리가 없을 경우 에러를 발생시키고 종료한다.


## 빌드 자동화
* 메이븐은 소프트웨어 빌드를 위한 공통 인터페이스를 제공하는 프레임워크
  + __플러그인 설정을 통해 기능을 위임__
* 빌드 단계(컴파일, 테스트, 패키징, 배포)들을 __빌드 라이프 사이클__ 이라고 한다.
* 각 빌드 단계에서 수행되는 작업을 __골(Goal)__ 이라고 한다.
* 실제 골은 그 단계에 연결된 __플러그인(Plugin)__ 에 의해 실행된다

![web_maven_3](/images/Web/web_maven_3.JPG)

### Maven 빌드 라이프사이클(lifecycle)
* Maven은 __기본, clean, site__ 3개의 라이프사이클이 있다.
* __기본 라이프사이클__ 은 여러 단계의 페이즈(Phase) 로 나뉘어져 있으며, 각 페이즈는 의존관계를 가진다. compile, test, package deploy 순서로 진행된다.
* __clean 라이프사이클__ 은 clean 페이즈를 이용하여 이전 빌드에서 생성된 모든 파일
(target 디렉토리)들을 삭제한다.
* __site 라이프사이클__ 은 site, site-deploy 페이즈를 이용하여 생성된 문서들을 대상 사이
트에 배포한다. 

### Maven Phase & Goal
* 빌드 라이프사이클은 하나 이상의 골을 수행하는 __페이지(Phase)__ 들로 구성.
* 각 페이즈 별로 플러그인이 작업을 수행한다. 이 작업을 __골(Goal)__ 이라고 한다.
  + 예) maven-compiler-plugin, maven-clean-plugin, maven-surefire-plugin
  + 플러그인: clean, compiler, surefire, jar, war, javadoc, antrun

![web_maven_4](/images/Web/web_maven_4.JPG)

# References
* 학부 강의 자료