## Spring 기본 개념
### Spring Bean의 생성과 관리

- 스프링 빈 이란?
  - 스프링 컨테이너에서 관리되는 객체로, 컨테이너에 의해 생명주기가 관리 되는 객체
```
스프링 컨테이너(Application Context)란?
Ioc컨테이너 중 스프링이 제공하는 구체적인 구현체로, Ioc컨테이너의 역할을 포함 함

Ioc(Inversion of Control) 란?
제어의 역전 즉, 메서드나 객체의 호출 작업을 개발자가 아닌 스프링에게 제어권을 넘기는 것
스프링 컨테이너에게 제어권을 넘겨 스프링 컨테이너가 흐름을 제어하게 됨
```
>[!Note]
> IoC컨테이너가 다른 언어에도 있는지?
> 
> C#에서도 IoC 컨테이너를 제공하고, 다른언어들에서는 정확히 IoC컨테이너를 지원하는건 아니지만 라이브러리 등을 통해 IoC컨테이너 역할을 구현할 수 있음
- 스프링 빈의 특징
  - 객체 생성 및 관리
    - 스프링 컨테이너가 객체를 대신 생성해 줌
  - 의존성 관리
    - TestService가 다른 객체를 필요로 할 때, 두 객체를 자동으로 연결해 줌
    - 즉, TestService가 다른 객체에 의존하는 경우 그 의존성을 해결해 줌
  - 객체 생명 주기 관리
    - **스프링 컨테이너 생성 -> Bean 생성 -> 의존성 주입 -> 초기화 콜백 -> Bean 사용 -> 소멸 전 콜백 -> 스프링 종료**의 과정
    >    - 초기화 콜백 : bean이 생성되고, bean의 의존성 주입이 완료된 뒤 호출
    >@PostConstruct 를 사용해서 초기화 메서드에 붙여줌
    >- 소멸 전 콜백 : 스프링이 종료되기 전, bean이 소멸되기 직전에 호출
    >  @PreDestory 를 사용해서 소멸 전 콜백 메서드에 붙여줌
- 스프링 컨테이너에서 빈 관리 방식
  - 스프링 컨테이너는 기본적으로 애플리케이션이 시작될 때 빈을 미리 생성하고, 그 후에는 필요할 때마다 빈을 제공함. 애플리케이션이 종료될 때 소멸
  - 스프링 컨테이너는 빈의 스코프에 따라 관리 방식을 다르게 설정할 수 있음
```
빈 스코프란?
빈이 존재하는 범위를 뜻 함
```
  - 싱글톤 스코프
    - 애플리케이션 내에서 딱 하나의 빈만 생성해서 사용하는 방식으로 기본적인 방식
    - 생성된 하나의 인스턴스는 single bean cache에 저장되며 해당 bean에 대한 요청과 참조가 있을 때마다 캐시된 객체를 반환<br>
      ![image](https://github.com/user-attachments/assets/749f2fba-262d-423e-aaae-0a8a3792d67a)<br>
    [이미지 출처-spring docs](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html, "spring docs")
  - 프로토타입 스코프
    - 요청이 있을 때마다 새로운 빈 생성하므로 하나의 Bean에 대해 다수의 객체가 존재할 수 있음
    - 컨테이너가 생성과 의존성 주입, 초기화까지만 관여
    - 요청마다 다른 값을 저장해야 하는 경우나, 사용자별로 독립적인 데이터를 관리해야 하는 경우, 각 요청마다 새로운 객체가 필요한 경우에 사용
    - @Scope 어노테이션을 @Bean 어노테이션과 함께 사용해야 함
    - 해당 빈에 대한 참조를 해제(ex =null)하면 GC 대상이 됨<br>
    ![image](https://github.com/user-attachments/assets/5354d77e-dc2b-4951-8556-3beebd24853b)<br>
    [이미지 출처-spring docs](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html, "spring docs")
> [!NOTE]
> 자바 객체를 생성해서 사용하는 것과 프로토타입 스코프를 이용해 빈으로 생성하는 것은 어떤 차이가 있나요?
>
> 스프링빈으로서의 기능이 필요하면 프로토타입 스코프, 필요없으면 객체생성

> [!NOTE]
> 프로토타입 스코프를 소멸하는 방법 중에 gc가 있는건지?
>
> 참조해제를 하면 gc의 대상이 되는건 맞지만, 대상이 된다고 해당 객체가 소멸된다는 보장은 없기 때문에 별도로 소멸해주는것이 좋음
- 스프링 빈 등록 방법
  - 어노테이션 기반 설정
    - 클래스에 @Component, @Service, @Repository, @Controller등을 사용하여 빈 등록
  - 자바 설정 파일
    - @Configuration과 @Bean 을 사용해 자바 **클래스**에 직접 빈 등록
    - 좀 더 명시적으로 빈을 설정하고 싶을 때 사용
>@Component : 
>스프링 빈으로 등록하기 위한 기본적인 어노테이션<br>
>@Service, @Controller, @Repository 등은 @Component를 확장한 형태로, 각 클래스가 어떠한 역할을 하는지에 대한 설명을 해주는 느낌
>
>@Autowired<br>
>스프링 빈을 자동으로 주입하는 어노테이션으로, 위의 어노테이션들이 붙은 빈들을 다른 클래스에서 사용할 수 있도록 해주는 역할
>
>@Bean<br>
>스프링 컨테이너에 수동으로 빈을 등록할 때 사용하는 어노테이션
>주로 @Configuration 클래스 내에서 사용됨
>외부 라이브러리의 클래스를 빈으로 등록할 때 사용하기 유용
>

> [!NOTE]
> @Component도 내부적으로는 @Bean으로 정의 되어있나요?
>
> 두 어노테이션은 역할은 비슷하지만 상속이나 포함 등의 관계는 아님
> ```java
> // @Component 정의
>@Target({ElementType.TYPE})
>@Retention(RetentionPolicy.RUNTIME)
>@Documented
>@Indexed
>public @interface Component {
>    String value() default "";
>}
> 
> ---------------------------------------------------------
>
>@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
>@Retention(RetentionPolicy.RUNTIME)
>@Documented
>public @interface Bean {
>    @AliasFor("name")
>    String[] value() default {};
>
>    @AliasFor("value")
>    String[] name() default {};
>
>    boolean autowireCandidate() default true;
>
>    String initMethod() default "";
>
>    String destroyMethod() default "(inferred)";
>}
>




- 스프링 빈을 사용하는 이유
  - 코드의 의존성을 줄이고 유지보수성을 높일 수 있음


https://docs.spring.io/spring-framework/reference/6.2-SNAPSHOT/core/beans.html
https://www.baeldung.com/spring-postconstruct-predestroy
https://docs.spring.io/spring-framework/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes
