
# 디스패처 서블릿(Dispatcher Servlet)


## 스프링의 구조

스프링은 크게 봤을 때, 아래와 같이 두개의 부분으로 나뉠 수 있습니다.
- 루트 웹 애플리케이션 컨텍스트(Root WebApplication Context): 웹 기술에서 완전히 독립적인 비즈니스 서비스 계층과 데이터 엑세스 계층을 포함
- 서블릿 애플리케이션 컨텍스트(Servlet WebApplicationContext): 스프링 웹 프레임워크와 관련된 빈을 포함

분리한 이유: 언제든 서블릿 애플리케이션 컨텍스트를 통째로 다른 기술로 대체할 수 있게 하기 위해


## 디스패처 서블릿이란?

- 스프링 MVC의 프론트 컨트롤러(front controller)
- 스프링의 웹 기술의 핵심이자 기반

> 스프링 MVC
> - 범용적인 웹 프레임워크
> - 프론트 컨트롤러 패턴과 함께 사용

### 프론트 컨트롤러 패턴

**정의**
- 웹 애플리케이션에서 모든 요청을 처리하는 컨트롤러 중앙 집중형 컨트롤러를 맨 앞에 두어, 들어오는 모든 요청을 받아 처리하는 패턴

**역할**
- 공통적인 작업을 먼저 수행한 후 적절한 페이지 컨트롤러로 작업을 위임한다.
- 클라이언트에게 보낼 뷰를 선택해 최종 결과를 생성하는 등의 작업을 수행한다.
- 예외 발생시 일관된 방식으로 처리한다.

> 프론트 컨트롤러(front controller)와 페이지 컨트롤러(page controller)
> - 프론트 컨트롤러: 모든 요청을 받아 처리하는 컨트롤러
> - 페이지 컨트롤러: 실제 비즈니스 로직을 수행하는 컨트롤러

### 디스패처 서블릿의 구조

![컨텍스트 간의 관계를 시각화](/java-spring/img/JAVA-SPRING-how_dispatcher_servle_work_dispatcher_servlet_architecher.png)
_컨텍스트 간의 관계를 시각화, 출처: [spring docs- DispatcherServlet](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet.html)_


- `DispatcherServlet`: 자기 자신의 컨텍스트인 `Servlet WebApplicationContext`를 포함한다.
- `Servlet WebApplicationContext`: 페이지 컨트롤러, 뷰 리졸버, 핸들러 매핑 등의 빈을 포함한다.
- `Root WebApplicationContext`: 서비스, 레포지토리 등의 빈을 포함한다.
- `Servlet WebApplicationContext`-`Root WebApplicationContext`는 자식-부모 관계로, 실제 디스패처 서블릿 내에 있지는 않다.


> spring의 부모-자식 관계
> - 애플리케이션 컨텍스트 간의 간계를 의미한다.
> - 자식 컨텍스트는 부모의 컨테스트의 빈을 가져다 사용할 수 있고, 반대의 경우는 불가능하다.
> - java의 부모-자식 관계(상속관계)와 전혀 다른 개념이다.

## 디스패처 서블릿의 요청 흐름
![디스패처 서블릿의 요청 흐름](/java-spring/img/JAVA-SPRING-how_dispatcher_servle_work_dispatcher_flow.png)
_디스패처 서블릿의 요청 흐름, 출처: [도서 - 토비의 스프링 3.1 vol.2](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)_

### 1. DispatcherServlet의 HTTP 요청 접수

```xml
<servlet-mapping>
    <servlet-name>Spring MVC</servlet-name>
    <url-pattern>/app/*</url-pattern>
</servlet-mapping>
```
_web.xml_

`web.xml`에는 디스패처 서블릿이 전달받을 URL의 패턴이 정의되어 있고, 이에 맞는 요청들이 디스패처 서블릿에게 전달된다.

디스패처 서블릿은 요청을 받아 공통 작업(보안, 파라미터 조작, 한글 디코딩 등)을 진행한다.

### 2. DispatcherServlet에서 컨트롤러로 HTTP 요청 위임

1. `HandlerMapping`을 이용해 작업을 위임할 컨트롤러를 결정한다.

- `HandlerMapping`은 URL이나 파라미터 정보, HTTP 명령등을 참고해 결정한다. 

2. `HandlerAdapter`를 이용해 위임할 컨트롤러에게 요청 작업을 전달한다.

![어댑터를 이용한 임의의 컨트롤러 호출 방식](image.png)
_어댑터를 이용한 임의의 컨트롤러 호출 방식, 출처: [도서 - 토비의 스프링 3.1 vol.2](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)_

- 어댑터 패턴을 이용해 디스패처 서블릿은 항상 일정한 방식으로 컨트롤러를 호출하고 결과를 받을 수 있다.

    디스패처 서블릿이 작업을 위임할 컨트롤러의 메서드를 실제 호출하기 위해서는 각 메서드의 서로 다른 호출 방법을 알아야하는데, 이때 `HandlerAdapter`를 이용하는 방식으로 호출한다.


> - 디스패처 서블릿은 서블릿 컨테이너(Servlet Container)내에 생성되기 때문에 스프링 컨테이너(Spring Container)에 직접 DI를 받는게 아니다.
> - 디스패처 서블릿이 생성될 때, `Servlet WebApplication Context`를 생성하고 연결해, DI를 받지 않지만 받는 것처럼 동작한다.

### 3. 컨트롤러의 모델 생성과 정보 등록


1. 사용자 요청을 해석
2. 서비스 계층에 작업을 위임
3. 모델을 생성
4. 어떤 뷰를 사용할지 결정

작업을 위임받은 컨트롤러의 위와 같은 작업을 통해, 모델과 뷰를 생성하고 이를 디스패처 서블릿에게 리턴한다.

### 4. 컨트롤러의 결과 리턴: 모델과 뷰

작업을 위임받은 컨트롤러가 모델과 뷰를 리턴할 때, `ModelAndView`라는 오브젝트로 주게된다. 

작업을 위임받았던 컨트롤러는 이 객체를 리턴해주며 작업이 끝난다.

### 5. DispatcherServlet의 뷰 호출

디스패처 컨트롤러는 이 `ModelAndView`를 받아 이후 작업을 진행한다.

1. 뷰 리졸버(View Resolver)를 이용해 뷰(View)의 논리적인 이름을 통해 뷰 오브젝트를 생성한다.

    > 뷰(View) 예시: jsp/jstl 등  

2. 뷰 오브젝트에게 모델을 전달해 클라이언트에게 돌려줄 최종 결과물을 생성해달라고 요청헌다.

    > 최종 결과물의 예시: HTML, JSON, RSS, Atom 등

### 6. 모델 참조

1. 뷰 오브젝트는 뷰 템플릿에 모델의 값을 넣어 최종 결과물을 만들어낸다.

    ```jsp
    <div>이름 : ${name}</div>
    ```
    이런 jsp 뷰 템플릿이 있다면, name부분을 모델에서 찾아 값을 넣어준다.

    같은 모델이더라도, 선택된 뷰에 따라 다른 형태의 결과물이 만들어진다.


2. 최종 결과물은 `HttpServletResponse` 오브젝트 내에 담긴다.

### 7. HTTP 응답 돌려주기

1. 등록된 후처리기가 있다면, 후처리기의 후속 작업을 진행한다.
2. 뷰가 만들어준 `HttpServletResponse`를 서블릿 컨테이너에게 돌려준다.

### 코드로 확인하기

다음은 github에 공개된 spring-framework `DispatcherServlet.java`의 일부분이다. 

- HTTP 데이터 요청시 
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            processedRequest = checkMultipart(request);
            multipartRequestParsed = (processedRequest != request);

            // 1. Handler 조회
            // 현재 요청에 대한 핸들러를 결정합니다.
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null) {
                noHandlerFound(processedRequest, response);
                return;
            }

            if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                return;
            }


            // 2. HandlerAdapter 결정
            // 핸들러 어댑터를 결정하고 핸들러를 호출합니다.
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

            // 3. 핸들러 호출 후 ModelAndView 리턴
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }

            applyDefaultViewName(processedRequest, mv);
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
            dispatchException = ex;
        }
        catch (Throwable err) {
            // 4.3부터는 핸들러 메서드에서 발생한 Error도 처리하고 있습니다.
            // 예외를 @ExceptionHandler 메서드 및 기타 시나리오에서 사용할 수 있도록 합니다.
            dispatchException = new ServletException("핸들러 디스패치 실패: " + err, err);
        }
        // 뷰 렌더링과 같은 후처리 작업
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    catch (Exception ex) {
        triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
    }
    catch (Throwable err) {
        triggerAfterCompletion(processedRequest, response, mappedHandler,
                new ServletException("Handler processing failed: " + err, err));
    }
    finally {
        if (asyncManager.isConcurrentHandlingStarted()) {
            // postHandle 및 afterCompletion 대신
            if (mappedHandler != null) {
                mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
            }
            asyncManager.setMultipartRequestParsed(multipartRequestParsed);
        }
        else {
            // 멀티파트 요청에서 사용된 모든 리소스를 정리합니다.
            if (multipartRequestParsed || asyncManager.isMultipartRequestParsed()) {
                cleanupMultipart(processedRequest);
            }
        }
    }
}
```
- 뷰 렌더링과 같은 후처리 작업
```java
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

    boolean errorView = false;

    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException mavDefiningException) {
            logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = mavDefiningException.getModelAndView();
        }
        else {
            Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
            mv = processHandlerException(request, response, handler, exception);
            errorView = (mv != null);
        }
    }

    // 4. 뷰 렌터링
    // 핸들러가 렌더링할 뷰를 반환했는가?
    if (mv != null && !mv.wasCleared()) {
        render(mv, request, response);
        if (errorView) {
            WebUtils.clearErrorRequestAttributes(request);
        }
    }
    else {
        if (logger.isTraceEnabled()) {
            logger.trace("No view rendering, null ModelAndView returned.");
        }
    }

    if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
        // 포워드 중 동시 처리 시작
        return;
    }

    if (mappedHandler != null) {
        // 예외(있는 경우)는 이미 처리되었습니다.
        mappedHandler.triggerAfterCompletion(request, response, null);
    }
}
```

## 디스패처 서블릿의 Special Bean Types

`Special Bean`이란, DispatcherServlet의 동작 방식과 기능을 확장, 변경할 수 있도록하는 Bean을 의미한다.

기본적으로 자주 사용되는 Bean들은 디폴트로 설정해주고 있기 때문에 필요한 부분만 확장해서 사용하고 나머지는 디폴트로 설정된 빈을 이용하면 된다.

조금 더 풀어서 얘기하자면, `DispatcherServlet`이 요청을 처리하며 이용되는 Bean의 종류가 다양하다.
스프링에서 각 bean을 기본으로 설정해 놓았지만 변경하고 싶다면 변경할 수 있다는 의미다.

### HandlerMapping

- URL과 요청정보를 기준으로, 어떤 컨트롤러를 사용할 적인지 결정하는 로직을 담당한다.

- 컨트롤러 전/후에 실행되는 `interceptor`를 연결한다.

- 매핑되는 기준을 각 HandlerMapping 구현에 따라 다르다.
    - `RequestMappingHandlerMapping`: `@RequestMapping`이 붙어있는 메서드를 매핑
    - `SimpleUrlHandlerMapping`: Bean 설정 파일(XML)에 URL 패턴과 핸들러를 직접 지정해 매핑


### HandlerAdapter

- 작업을 위임할 컨트롤러의 메서드의 호출 방법에 관계없이 호출 할 수있도록 도와준다.

    - `DispatcherServlet`은 `ServletContainer`에 속해있기 때문에 `SpringContainer`에 속해 있는 컨트롤러의 메서드 호출방법을 알 방법이 없다.

    - 혹시 알 수 있다고 해도, 서로 다른 컨테이너에서 관리되기 때문에 직접 호출할 방법이 없다.

    - 이를 `HandlerAdapter`가 컨트롤러를 호출하는 방법을 캡슐화하여 `DispatcherServlet`이 컨트롤러를 직접 호출하지 않고도 작업을 위임할 수 있도록 해주어서 해결한다.

- `DispatcherServlet`이 항상 같은 방식으로 작업을 위임하고, 같은 형식의 결과를 받을 수 있도록 해준다.

### HandlerExceptionResolver

- 예외가 발생했을 때 이를 처리하는 로직을 가지고 있다.
- `HandlerExceptionResolver`는 여러 종류가 있고, 발생한 예외에 따라 이 중에서 하나에게 예외 처리를 위임한다.

    - `@ExceptionHandler`: 컨트롤러 내부에서 특정 예외를 처리할 수 있는 메서드를 명시
    
        ```java
            @Controller
            public class MyController {
                @ExceptionHandler(NullPointerException.class)
                public String handleNullPointer(Exception ex) {
                    return "errorPage";
                }
            }
        ```

    - `@ControllerAdvice`: 전역 예외 처리를 담당하는 어노테이션
        ```java
            @ControllerAdvice
            public class GlobalExceptionHandler {
                @ExceptionHandler(RuntimeException.class)
                public String handleRuntimeException(RuntimeException ex) {
                    return "globalErrorPage";
                }
            }
        ```

    - `HandlerExceptionResolver`: Spring MVC의 기본 예외 처리 전략으로, 예외와 뷰를 매핑하여 처리

### ViewResolver

- 논리적 문자열의 뷰 이름을 기반으로, 적절한 뷰 오브젝트로 찾아준다. 

- 스프링이 지원하는 뷰의 종류는 매우 다양하고, 뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해 줄 수 있다.


### LocaleResolver, LocaleContextResolver

- SpringMVC의 국제화(i18n)를 지원하기 위해 사용되는 구성 요소다.

- 클라이언트가 사용하는 지역(Locale)과 타임존(TimeZone)을 결정해 국제화 된 뷰를 제공한다.
    - HTTP 헤더의 정보를 보고 지역정보를 설정해준다.

### ThemeResolver

- 테마를 가지고 이를 변경해서 사이트를 구성할 경우, 테마 정보를 결정해준다.

### MultipartResolver

- 멀티파트 요청(Multipart Request)을 처리한다.
- 기본적으로 Servlet API는 멀티파트 요청을 파일을 포함한 요청을 직접 파싱해 처리해야하는데, 추상화하여 쉽게 처리할 수 있도록 만들어준다.

### FlashMapManager

- 리다이렉트(redirect) 간에 데이터를 전달하는 역할이다.
- 리다이렉트 후에 데이터를 유지할 때 사용된다.

### 출처
- [spring docs- DispatcherServlet](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet.html)
- [wikipedia - Spring Framework](https://en.wikipedia.org/wiki/Spring_Framework#Configuration_of_DispatcherServlet)
- [wikipedia - front controller](https://en.wikipedia.org/wiki/Front_controller)
- [도서 - 토비의 스프링 3.1 vol.2](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)
- [github - springframework의 DispatcherServlet.java](hhttps://github.com/spring-projects/spring-framework/blob/main/spring-webmvc/src/main/java/org/springframework/web/servlet/DispatcherServlet.java)