# ApplicationContext : 스프링 컨테이너

### 스프링 컨테이너란?
> 스프링에서 빈을 생성하고 관리하는 역할
> * 빈(bean) : 객체

### 생성방법
```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class) 
```
여기서 파라미터로 넘겨주는 ***AppConfig***는 앱의 ***구성정보***를 나타내는 클래스이고 이 정보를 이용하여 스프링 프레임워크가 빈을 생성하고 관리한다. </br>
해당 클래스는 앞에 ***@Configuration*** 이라는 어노테이션을 붙여준다.
