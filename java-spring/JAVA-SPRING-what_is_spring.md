# 스프링(Spring)이란?

"스프링(Spring)"은 스프링 DI 컨테이너의 기술, 스프링 생태계, 스프링 프레임워크 등으로 여러가지 의미를 가지고 있습니다.

이 글에서는 spring framework를 중심으로 합니다.

> **What We Mean by "Spring"**
>
> - The term "Spring" means different things in different contexts. It can be used to refer to the Spring Framework project itself, which is where it all started. Over time, other Spring projects have been built on top of the Spring Framework. Most often, when people say "Spring", they mean the entire family of projects.
> - “Spring”이라는 용어는 문맥에 따라 다른 의미를 가질 수 있습니다. 가장 처음 시작된 Spring Framework 프로젝트를 가리킬 때 사용될 수 있습니다. 시간이 지나면서 Spring Framework 위에 다른 Spring 프로젝트들이 구축되었습니다. 대부분 사람들이 “Spring”이라고 말할 때는 전체 Spring 프로젝트 가족을 의미합니다.

_참조: [spring docs - What We Mean by "Spring"](https://docs.spring.io/spring-framework/reference/overview.html#overview-spring)_

## spring의 개요

### spring 배경과 특징

- 시작
  - J2EE와 EJB를 비판하기 위해 작성된 책(J2EE 설계와 개발, 로드 존슨)의 예제코드(3만줄)에서 시작했습니다.

- 특징
  - 엔터프라이즈 시스템의 복잡함을 분리
    - 비즈니스적 복잡함: VIP회원과 일반회원을 나눠서 관리 등
    - 기술적 복잡함: DB 사용시 트랜젝션을 이용 등

    > 엔터프라이즈 시스템
    > - 서버에서 동작하며 기업과 조직의 업무를 처리해주는 시스템
    > - 비즈니스 로직은 원래 복잡하지만, 요즘에는 비즈니스의 변경이 잦아져서 복잡도가 올라갔습니다.

  - 경량급
    - (EJB보다) 가볍다는 의미를 가집니다.
    - 실제로는 무겁지만 그것이 불필요하게 무겁지 않다는 의미도 있습니다.
  - 오픈소스
    - 오픈소스이기 때문에 다양한 사람들이 빠르게, 많은 테스트가 가능합니다.
    - 오픈소스의 단점(개발자 개인이 진행하기 때문에 유지가 힘듦)을 기업에서 개발을 책임져 극복했습니다.
  - POJO 프레임워크
    - POJO(Plain Old Java Object): 간단한 자바 오브젝트
    - POJO 프레임워크
      - POJO 프로그래밍이 가능하도록 기술적인 기반을 제공해주는 프레임워크
      - 비즈니스 로직과 기술의 복잡함을 분리하고, 비즈니스 로직에는 기술적 복잡함의 모습을 감추는 프레임워크

- 스프링의 기술
  - Ioc/DI
  - AOP
  - PSA
  
### Spring의 핵심

핵심 : **"객체 지향"**

> - 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
>
> _[도서 - 토비의 스프링 3.1 vol.2](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)_

> - 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크
> - 객체 지향 언어가 가진 강력한 특징을 살려내는 프레임워크
>
> _[youtube 김영한 - 02. 스프링이란](https://www.youtube.com/watch?v=q_QEh3fz0zw)_

> - Spring Framework는 Java 애플리케이션 개발을 위한 포괄적인 인프라 지원을 제공하는 Java 플랫폼
>
> - Spring을 사용하면 "Plain Old Java Objects"(POJO)에서 애플리케이션을 빌드하고 POJO에 비침습적으로 엔터프라이즈 서비스를 적용할 수 있습니다.
>
> [spring docs - spring framework 소개](https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/overview.html)

**=> 좋은 객체 지향 프로그래밍이 무엇인지 알아야 더욱 진가를 알 수 있다.**

### Spring 생태계

스프링은 스프링 생태계를 가지고 있습니다.

스프링 생태계에서는 spring framework, spring boot, spring data, spring session, spring sequrity, spring batch 등이 포함됩니다.

이번엔 이 생태계에 대해 알아보는 것이 아니니, 스프링 생태계에는 여러가지가 포함된다는 것 까지만 확인하고 넘어가도록 하겠습니다.

### Spring framework과 Spring boot

흔히 혼동하는 개념으로, 스프링 프레임워크를 잘 알기 위해서는 스프링 부트와 잘 구분해야합니다.

- spring framework
  - spring의 핵심 기술로, 실제 프레임워크를 의미한다.
  - spring 기술 사용시 필수적으로 사용되어야합니다.
- spring boot
  - spring framework를 더 쉽게 사용하도록 도와줍니다.
  - 스프링 생태계에서 다른 기술을 추가로 사용시 편리하게 사용하도록 도와줍니다.

스프링 기술을 사용해서 개발을 진행할 때 spring framework만 이용해도되고 이전에는 그렇게 개발했지만,
요즈음에는 스프링 부트를 기본적으로 사용하는 경우가 많습니다.

스프링 프레임워크는 프레임워크지만 실제 사용하기에 번거로운 부분들이 많이 때문에 이를 스프링 부트가 개발이 좀 더 빠르고 편리할 수 있도록 해줍니다.

#### 특징 비교

> - spring framework
>   - **핵심 기술** : 종속성 주입, 이벤트, 리소스, i18n, 검증, 데이터 바인딩, 유형 변환, SpEL, AOP
>   - **테스트** : 모의 객체, TestContext 프레임워크, Spring MVC 테스트 WebTestClient
>   - **데이터 접근** : 트랜잭션, DAO 지원, JDBC, ORM, XML 마샬링
>   - **웹 프레임워크** : Spring MVC 와 Spring WebFlux
>   - **통합** : 원격, JMS, JCA, JMX, 이메일, 작업, 스케줄링, 캐시 및 관찰성
>   - **언어** : Kotlin, Groovy, 동적 언어
> - spring boot
>   - 독립 실행형 Spring 애플리케이션 생성
>   - Tomcat, Jetty 또는 Undertow를 직접 내장합니다(WAR 파일을 배포할 필요가 없습니다)
>   - 빌드 구성을 단순화하기 위해 의견이 있는 '스타터' 종속성을 제공합니다.
>   - 가능한 경우 Spring 및 타사 라이브러리를 자동으로 구성합니다.
>   - 메트릭, 상태 검사, 외부화된 구성과 같은 프로덕션에 적합한 기능 제공
>   - 코드 생성이 전혀 없고 XML 구성이 필요하지 않습니다.

_참조: [spring.io - spring boot](https://spring.io/projects/spring-boot), [spring.io - spring framework](https://spring.io/projects/spring-framework)_

이처럼 spring framework는 실제 기능을 제공하고, spring boot는 spring framework 위에서 라이브러리 종속성 해결과 was 내장, xml을 통한 설정등을 편리하게 하도록 하는 것으로 서로 다른 목표를 가지고 있습니다.

## Spring framework

이제 본격적으로 spring framework에 대해 알아봅시다.

> spring 3.0을 기준으로 설명합니다.
> spring을 깊숙히 알기 위해서는, 많은 기능이 있는 현재의 버전보다 예전 버전으로 설명하는 것이 좋을 것이라고 생각했기 때문입니다.

### Inversion of Control (IoC)와 Dependency Injection (DI)

- IoC는 객체의 생성 및 의존성 관리 방식을 개발자가 아닌 컨테이너가 제어하는 원리입니다.
- DI는 IoC의 한 형태로, 객체의 의존성(다른 객체들)을 외부에서 주입하는 방식입니다.

Spring은 IoC를 통해 객체의 생성과 의존성을 관리하여, 개발자가 객체 간의 의존 관계를 직접 관리할 필요 없이, 프레임워크가 이를 대신 처리하도록 합니다.

### Spring framework Module

Spring frameworks는 다양한 기능을 제공하는 여러 개의 모듈로 구성되어 있습니다.

![spring framework의 구조](/java-spring/img/JAVA-SPRING-what_is_spring_module.png)

_[spring docs - spring framework 소개](https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/overview.html)_

모듈
- Core Container
  - 역할
    - Spring의 가장 기본적인 구성 요소
    - 애플리케이션의 객체 생성과 의존성 관리를 담당(IoC와 DI 기능)
  - 기능
    - Core
      - Spring Framework의 핵심 모듈
      - BeanFactory라는 객체를 이용해 객체 생성 및 의존성 주입
    - Beans
      - BeanFactory를 확장하여 더 강력한 기능과 객체의 생명주기를 관리
    - Context
      - ApplicationContext를 제공해, 애플리케이션의 객체들을 관리
      - ApplicationContext 인터페이스는 다양한 기능을 제공(이벤트 전달, 리소스 로딩 등)
    - Expression Language(EL)
      - 객체의 프로퍼티 값에 접근하고, 메서드 호출을 수행할 수 있는 표현식 언어를 제공
      - e.g. `@Value`와 같이 동적 값 주입에 사용
- Data Access/Integration
  - 역할: 데이터베이스 연동 및 트랜잭션 관리 기능을 제공한다.
  - 기능: JDBC, ORM, OXM, JMS, Transaction
- Web
  - 역할: 웹 애플리케이션 개발을 위한 기능 제공한다.
  - 기능
    - Web
      - Spring의 웹 애플리케이션 개발을 위한 기본적인 통합 기능 제공
      - e.g. HTTP 요청 처리, 파일 업로드 처리 등
    - Servlet
      - MVC 프레임워크 제공
      - 클라이언트의 요청을 처리하고 적절한 뷰를 반환하는 구조 제공
    - Portlet
      - 포틀릿 환경에서 사용할 수 있는 MVC 구현을 제공
    - Struts(제공 중단)
      - 기존의 Struts 프레임워크와 통합할 수 있도록 지원하는 모듈
- AOP (Aspect-Oriented Programming), Aspects, Instrumentation
  - 역할
    - Aspect-Oriented Programming을 지원한다.
    - 비즈니스 로직과 다른 기능을 분리하여 코드의 재사용성과 유지보수성을 높인다.
  - 기능
    - AOP
      - AOP Alliance 규격을 준수하는 AOP 구현을 제공
    - Aspects
      - AspectJ와 통합할 수 있는 모듈을 제공
      - 컴파일 타임에 AOP 적용, 더 복잡한 포인트컷 표현식 등
    - Instrumentation
      - 특정 애플리케이션 서버에서 사용되는 클래스 계측 지원 및 클래스 로더 구현을 제공
- Test
  - 역할:Spring 애플리케이션의 테스트를 지원한다.
  - 기능: JUnit, TestNG, Mock 등

### Spring Framework Container

> 이 정보는 공식적인 정보에서는 찾지 못한 정보입니다.

- 서블릿 컨테이너(`Servlet Container`)
  - WAS가 생성하고, 관리하는 컨테이너
  - 서블릿의 생명 주기를 관리하고, HTTP 요청을 받고 응답을 처리

- 스프링 컨테이너(`Spring Container`)
  - `WebApplicationContext`
    - `Servlet Container`와 연동되어 `DispatcherServlet`을 통해 웹요청을 컨텍스트내에 있는 빈을 통해 처리
    - 웹 관련 로직(컨트롤러, 뷰 리졸버 등)을 담당
    - `RootApplicationContext`의 확장한 것이 `WebApplicationContext`으로, 위의 Spring Framework Moduled의 그림에서 본 Web module을 포함
    - 웹 요청 처리 외에도 비즈니스 로직을 처리하는 빈들을 `RootApplicationContext`에서 가져와 사용
    - WAS에 따라 설정이나 동작 방식이 변경 될 수도 있습니다.
  - `RootApplicationContext`
    - 비웹 애플리케이션에서 사용되는 빈들(예: 비즈니스 로직, 데이터베이스 연동, 서비스 계층 등)을 관리
    - 위의 Spring Framework Moduled의 그림에서 본 Web module 제외한 나머지를 포함
    - WAS에 따라 설정이나 동작 방식이 변경되지 않습니다.

#### Spring Framework DI

![Spring Framework DI](/java-spring/img/JAVA-SPRING-what_is_spring_di.png)

_Spring Framework DI, [kouzie.github.io blog - Spring 개요](https://kouzie.github.io/temp/springFramework/2019-06-10-Spring%20-%20%EA%B0%9C%EC%9A%94)_

#### 참조

- [youtube 김영한 - 02. 스프링이란](https://www.youtube.com/watch?v=q_QEh3fz0zw)
- [도서 - 토비의 스프링 3.1 vol.2](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)
- [spring docs - spring framework 소개](https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/overview.html)
- [kouzie.github.io blog - Spring 개요](https://kouzie.github.io/temp/springFramework/2019-06-10-Spring%20-%20%EA%B0%9C%EC%9A%94)