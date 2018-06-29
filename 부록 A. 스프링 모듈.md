# 스프링 모듈
스프링 프레임워크는 20개의 작은 모듈로 세분화되어 있다. 스프링을 사용하는 목적과 환경, 사용 기술에 따라 필요한 모듈을 선택해 사용해야 한다. 또, 모듈 사이의 의존관계도 이해할 필요가 있다.

모듈의 의존관계는 필수 의존관계와 선택 의존관계가 있다. 모듈 A가 모듈 B에 대해서 필수 의존관계를 갖고 있다면 모듈 A를 사용할 때는 모듈 B를 반드시 포함시켜야 한다. 
반면에 선택 의존관계를 가진 모듈도 있다. 모듈 A가 모듈 C에 대해 선택 의존관계를 갖고 있다면 모듈 A를 사용하기 위해 모듈 C가 반드시 필요한 것은 아니지만, 
모듈 A의 특정 기능을 사용할 때는 모듈 C가 필요할 수도 있다는 뜻이다. 따라서 선택적인 의존관계에 있는 모듈에 대해서는 어떤 경우에 필요한지 알고 있어야 한다.
<hr/>

### 스프링 모듈의 종류와 특징
#### 스프링 모듈 이름
스프링 모듈은 jar 확장자를 가진 파일이다. 모듈 파일의 이름은 명명 규칙에 따라 두 가지로 만들어져 있다. 스프링 모듈은 기본적으로 OSGi의 모듈 명명 규칙을 따라서 
다음과 같이 패키지 이름과 모듈 버전으로 구성된다.
```
org.springframework.core-3.0.7.RELEASE.jar
        모듈 패키지 이름 - 모듈 버전
```
스프링 모듈은 OSGi의 모듈 조건을 충족하는 OSGi 번들이기도 하다. 따라서 OSGi 플랫폼에 바로 가져다 사용할 수 있다. 물론 OSGi가 아닌 일반 JavaEE나 JavaSE 
환경에서도 아무런 문제 없이 사용할 수 있다.

Maven의 명명 규칙을 따라 만들어진 스프링 모듈 파일도 있다. Maven이나 Ivy를 통해 접근할 수 있는 Maven Central 리포지토리에서는 다음과 같은 Maven 스타일의 
모듈 이름을 가진 파일을 찾을 수 있다. Maven 모듈 이름에는 패키지를 사용하지 않기 때문에 이름이 단순한 편이다.
```
spring-core-3.0.7.RELEASE.jar
  모듈 이름 - 모듈 버전
```
> 이 두 개의 파일은 이름은 다르지만 그 내용은 완전히 동일하다.
#### 스프링 모듈 추가
스프링 모듈을 프로젝트에 추가하는 방법은 두 가지가 있다. 스프링 배포판에 포함된 모듈을 직접 추가하는 방법과 Maven이나 Ivy의 의존 라이브러리 관리 기능을 이용해 모듈 리포지토리에서 자동으로 추가되게 하는 방법이다.
##### 수동 추가
스프링 모듈을 얻을 수 있는 가장 손쉬운 방법은 스프링 배포판을 이용하는 것이다. 스프링 프레임워크 배포판은 http://www.springsource.com/download/community에서 다운로드할 수 있다. 배포판 파일의 압축을 풀고 dist 폴더를 열어보면 스프링 모듈을 찾을 수 있다.

스프링 배포판에 포함된 모듈 파일은 OSGi 호환 모듈 이름을 갖고 있다. 사용할 기능에 따라 적절한 모듈파일을 프로젝트에 포함시켜주면 된다. 수동으로 모듈을 추가할 때는 모듈 의존관계에 따라서 필요한 의존모듈을 빼먹지 않도록 주의해야 한다.
##### Maven/Ivy 자동 추가
Maven을 이용해 의존 라이브러리를 관리한다면 pom.xml 파일의 의존정보 설정만으로 필요한 스프링 모듈을 가져올 수 있다.

스프링 모듈은 Maven Central 리포지토리에 등록되어 있다. 따라서 리포지토리를 따로 지정하지 않아도 스프링 모듈을 가져올 수 있다. 이때는 Maven 스타일의 모듈 이름을 사용해서 스프링 모듈을 가져와야 한다.
* core 모듈에 대한 Maven 의존 라이브러리 선언 항목
```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>3.0.4.RELEASE</version>
</dependency>
```
gruopId는 항상 동일하다. artifactId는 스프링 모듈의 Maven 이름으로 지정해주면 된다.

OSGi 이름을 가진 모듈을 사용하려면 스프링 소스에서 제공하는 Maven 리포지토리를 다음과 같이 지정해줘야 한다.
```
<repository>
        <id>com.springsource.repository.bundles.release</id>
        <name>springSource Enterprise Bundle Repository - springSource Bundel Releases</name>
        <url>http://repository.springsource.com/maven/bundles/release</url>
</repository>
```
스프링소스 리포지토리를 지정했다면 다음과 같이 OSGi 이름을 가진 모듈을 가져올 수 있다.
```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>3.0.4.RELEASE</version>
</dependency>
```
Maven Central에 등록된 모듈을 사용할 때와 다른 점은 artifactId에 OSGi 모듈 이름을 사용하는 것이다.

Maven을 이용하면 전이적 의존관계 관리기능이 적용되므로 필수 의존관계에 있는 모듈이 자동으로 추가된다. 따라서 모든 모듈을 일일이 지정할 필요가 없다.
#### 스프링 모듈 목록
모듈 이름 | OSGi 모듈 이름 | Maven 모듈 이름 | 주요 기능
:--------:|:------------:|:--------------:|:----------
AOP | org.springframework.aop | spring-aop | AOP
ASM | org.springframework.asm | spring-asm | ASM 재패키징
Aspects | org.springframework.aspects | spring-adpects | AspectJ 지원
Beans | org.springframework.beans | spring-beans | 빈 팩토리
Context | org.springframework.context | spring-context | 애플리케이션 컨텍스트
Context.Support |org.springframework.context.support | spring-context-support | 컨텍스트 부가 기능
Core | org.springframework.core | spring-core | 공통 기능
Expression | org.springframework.expression | spring-expression | SpEL
Instrument | org.springframework.instrument | spring-instrument | 스프링 Java Agent
Instrument.Tomcat | org.springframework.instrument.tomcat | spring-instrument-tomcat | 톰캣 클래스 로더
JDBC | org.springframework.jdbc | spring-jdbc | JDBC
JMS | org.springframework.jms | spring-jms | JMS
ORM | org.springframework.orm | spring-orm | ORM(Hibernate, JPA 등)
OXM | org.springframework.oxm | spring-oxm | OXM
Test | org.springframework.test | spring-test | 테스트
Transaction | org.springframework.transaction | spring-tx | 트랜잭션
Web | org.springframework.web | spring-web | 웹 공통
Web.Portlet | org.springframework.web.portlet | spring-webmvc-portlet | 포틀릿
Web.Servlet | org.springframework.web.servlet | spring-webmvc | 서블릿
Web.Struts | org.springframework.web.struts | spring-struts | 스트럿츠 1 지원
<hr/>

### 스프링 모듈의 의존관계


