
서블릿이 예외 처리를 지원하는 방식

1. Exception
2. response.sendError(HTTP 상태코드, 오류 메시지)

Exception

- 사용자 요청을 처리 중 Exception이 발생하면 WAS 까지 전달된다. 그러면 WAS(tomcat)이 기본으로 제공하는 오류 화면을 볼 수 있다.

response.sendError(HTTP 상태코드, 오류 메시지)

- 서블릿 컨테이너에게 오류가 발생했다는 점을 전달

서블릿 컨테이너가 제공하는 기본 예외 처리 화면은 고객 친화적이지 않음 

⇒ 오류 화면 직접 만들기

WebServerFactoryCustomizer

- Strategy interface for customizing web server factories. Any beans of this type will get a callback with the server factory before the server itself is started, so you can set the port, address, error pages etc. ⇒ 이 인터페이스를 구현한 클래스는 서버 자체가 시작되기전에 서버 팩토리와 함께 콜백을 받으므로 port, address, error pages 등을 설정할 수 있다.
- @Component 빈으로 등록해준다.

```java
@Component
public class WebServerCustomizer implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {

    @Override
    public void customize(ConfigurableWebServerFactory factory) {

        ErrorPage errorPage404 = new ErrorPage(HttpStatus.NOT_FOUND, "/error-page/404");
        ErrorPage errorPage500 = new ErrorPage(HttpStatus.INTERNAL_SERVER_ERROR, "/error-page/500");
        ErrorPage errorPageEx = new ErrorPage(RuntimeException.class, "/error-page/500");

        factory.addErrorPages(errorPage404, errorPage500, errorPageEx);
    }
}
```

ErrorPage

```java

	public ErrorPage(HttpStatus status, String path) {
	}

	public ErrorPage(Class<? extends Throwable> exception, String path) {
	}
```
