# 스프링 의존 라이브러리
스프링 3.0과 스프링 3.1에는 스프링 모듈 외에도 100여 개의 의존 라이브러리가 존재한다. 이 의존 라이브러리는 스프링 프레임워크를 빌드하고 테스트하는 데 필요한 라이브러리다. 
스프링 애플리케이션을 개발하고 운영할 때는 이 외에도 다양한 많은 라이브러리가 추가로 필요할 수 있다.

스프링의 의존 라이브러리는 스프링의 업데이트마다 새롭게 추가되거나 버전이 바뀔 수 있다. 최신 의존 라이브러리 정보는 스프링 배포판이나 Maven 리포지토리의 POM 정보를 참고.
<hr/>
### 의존 라이브러리의 종류와 특징
#### 의존 라이브러리 이름
의존 라이브러리는 스프링 개발팀이 만든 것이 아니므로 모듈의 이름에 일정한 패턴이 있지는 않다. 하지만 스프링 소스가 OSGi 호환 모듈로 재패키징한 모듈의 이름은 일정한 패턴이 있다.

최신 아파치 Commons 프로젝트(http://commons.apache.org)의 Logging 라이브러리 파일의 이름은 commons-logging-1.1.1.jar이다. Maven 리포지토리에 등록된 라이브러리 파일 이름도 동일하다.

그런데 스프링소스는 Commons Logging 라이브러리 파일을 OSGi 표준에 맞도록 메타정보를 추가해서 재패키징하고, OSGi 모듈의 명명 규칙을 따라 다음과 같은 이름의 파일로 만들어 제공한다.
```
com.springsource.org.apache.commons.logging-1.1.1.jar
공통 패키지 이름       모듈 이름               버전
```
OSGi의 명명 규칙을 따라 모듈의 기본 패키지 이름을 모듈 이름으로 사용하게 했을뿐만 아니라, 스프링소스가 OSGi 플랫폼에서 사용 가능한 모듈로 재패키징했음을 나타내도록 
항상 com.springsource라는 공통 패키지 이름이 추가되어 있다.

오픈소스 프레임워크나 라이브러리뿐 아니라 자바의 표준 API를 위해서도 OSGi 호환 모듈이 제공된다. 예를 들어 서블릿 API는 보통 servlet-api.jar라는 이름의 
파일로 되어 있다. 반면에 스프링소스가 제공하는 OSGi 호환 라이브러리 이름은 com.springsource.javax.servlet-2.5.0.jar이다. 앞의 com.springsource 부분을 
제외하면 모듈의 패키지 이름을 알 수 있으므로 어떤 라이브러리 파일인지 쉽게 확인할 수 있을 것이다. 표준 API는 보통 서버에서 제공되기 때문에 애플리케이션 모듈에 포함시킬 
필요는 없지만, 빌드 작업 중에 필요하기 때문에 애플리케이션 프로젝트에 포함시켜야 한다.

스프링 모듈은 파일의 이름이 달라도 내용은 동일하다. 반면에 스프링 소스에 의해서 OSGi 호환 라이브러리로 재패키징된 파일은 라이브러리의 공식 배포파일이나 Maven에 등록된 파일과 구성이 조금 다
다를 수 있다는 점에 주의해야 한다.
#### 의존 라이브러리 추가
스프링 프로젝트에 의존 라이브러리를 추가하는 방법은 스프링 모듈과 마찬가지로 수동으로 라이브러리 파일을 직접 넣어주는 방법과, Maven 또는 Ivy의 의존 라이브러리 
관리 기능을 사용해 자동으로 넣어주는 방법이 있다.
##### 수동 추가
스프링의 의존 라이브러리 파일은 각 라이브러리의 웹사이트에서 직접 다운로드 받을 수 있다. 라이브러리의 웹사이트나 다운로드 정보는 검색엔진 등을 통해 직접 찾아야 한다. 
이때는 스프링 버전에 따라서 호환 가능한 의존 라이브러리 버전이 맞는지 확인해야 한다. 버전이 일치하지 않으면 바르게 동작하지 않을 수도 있다.

스프링소스가 제공하는 OSGi 호환 라이브러리 파일을 사용할 경우에는 스프링소스의 리포지토리 검색 기능을 활용하면 된다.

스프링 소스 엔터프라이즈 번들 리포지토리의 검색 기능(http://ebr.springsource.com/repository/app/)을 이용해 라이브러리를 찾으면 된다.

각 라이브러리의 의존정보는 해당 라이브러리의 문서나 POM 정보를 참고. 수동 추가의 경우는 이렇게 추가적으로 필요한 의존 라이브러리를 일일이 찾아서 넣어줘야 하는 번거로움이 있다.
##### 자동 추가
Maven이나 Ivy를 이용하는 경우에는 Maven의 디폴트 리포지토리인 Maven Central에서 Maven 스타일의 이름을 가진 파일을 가져오는 방법과 스프링소스가 제공하는 Maven 리포지토리에서 OSGi 호환 파일을 가져오는 방법이 있다.

디폴트 리포지토리를 이용하는 경우에는 Maven 검색 사이트(http://mvnrepository.com/)를 통해 손쉽게 의존 라이브러리 설정정보를 얻을 수 있다.

Commons Logging이라면 다음과 같은 ```<dependency>``` 태그를 pom.xml에 넣어주면 된다.
```
<dependency>
  <groupId>commons-logging</groupId>
  <artifactId>commons-logging</artifactId>
  <version>1.1.1</version>
</dependency>
```
스프링소스가 제공하는 OSGi 호환 라이브러리 Maven 리포지토리를 이용하려면 먼저 다음과 같은 추가 리포지토리 설정을 pom.xml에 넣어줘야 한다.
```
<repository>
  <id>com.springsource.repository.bundles.external</id>
  <name>SpringSource Enterprise Bundle Repository - External Bundle Releases</name>
  <usl>http://repository.springsource.com/maven/bundles/external</url>
</repository>
```
스프링소스 Maven 리포지토리에 등록된 Commons Logging은 다음과 같은 설정을 통해 가져올 수 있다. 
