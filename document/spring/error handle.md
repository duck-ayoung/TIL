
### 서블릿이 예외 처리를 지원하는 방식

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


### 스프링 부트가 지원하는 방식
* ErrorPage를 자동으로 등록한다. 이때 /error라는 경로로 기본 오류 페이지를 설정한다.
* BasicErrorController라는 스프링 컨트롤러를 자동으로 등록한다.

### BasicErrorController

- Basic global error @Controller, rendering ErrorAttributes. More specific errors can be handled either using Spring MVC abstractions (e.g. @ExceptionHandler) or by adding servlet server error pages. ⇒ 기본 global error Controller
- ErrorMvcAutoConfiguration에서 errorPage등록하고 BasicErrorController는 처리하는 controller

```java
//ErrorMvcAutoConfiguration
@Override
		public void registerErrorPages(ErrorPageRegistry errorPageRegistry) {
			ErrorPage errorPage = new ErrorPage(
					this.dispatcherServletPath.getRelativePath(this.properties.getError().getPath()));
			errorPageRegistry.addErrorPages(errorPage);
		}
```

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
}
```


### API 오류 처리 방법

1. error controller에서 JSON 데이터를 반환하여 처리
    
    ```java
    @RequestMapping(value = "/error-page/500", produces = MediaType.APPLICATION_JSON_VALUE)
        public ResponseEntity<Map<String, Object>> errorPage500Api(HttpServletRequest request, HttpServletResponse response) {
            log.info("API errorpage 500");
    
            Map<String, Object> result = new HashMap<>();
            Exception ex = (Exception) request.getAttribute(ERROR_EXCEPTION);
            result.put("status", request.getAttribute(ERROR_STATUS_CODE));
            result.put("message", ex.getMessage());
    
            Integer statusCode = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
    
            return new ResponseEntity<>(result, HttpStatus.valueOf(statusCode));
        }
    ```
    
    produce에 값을 지정하면 클라이언트가 요청하는 Accept가 지정한 값과 같으면 이 메서드가 호출된다. (따라서 지정하지 않은 같은 매핑주소를 가진 메서드는 그 이외의 상황에 불릴 것 이다.)
    
    RequestMapping - produce 
    
    - Narrows the primary mapping by media types that can be produced by the mapped handler. Consists of one or more media types one of which must be chosen via content negotiation against the "acceptable" media types of the request. Typically those are extracted from the "Accept" header but may be derived from query parameters, or other. <br>Examples:
<br>    produces = "text/plain"
<br>    produces = {"text/plain", "application/*"} //여러개 지정 가능
<br>    produces = MediaType.TEXT_PLAIN_VALUE
<br>    produces = "text/plain;charset=UTF-8"

- Spring에서 제공하는 BasicErrorController에서의 처리 방식은?
    
    ```java
    //MediaType.TEXT_HTML_VALUE 인 경우에 ModelAndView 반환
    @RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
    	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
    		HttpStatus status = getStatus(request);
    		Map<String, Object> model = Collections
    				.unmodifiableMap(getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
    		response.setStatus(status.value());
    		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
    		return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
    	}
    
    //그 외에는 JSON data로 반환
    	@RequestMapping
    	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
    		HttpStatus status = getStatus(request);
    		if (status == HttpStatus.NO_CONTENT) {
    			return new ResponseEntity<>(status);
    		}
    		Map<String, Object> body = getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.ALL));
    		return new ResponseEntity<>(body, status);
    	}
    ```
