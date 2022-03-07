### 스프링의 타입 변환에서 새로운 타입을 정의하는 방법

1. Converter 인터페이스 상속 받아 구현
    
    Type parameters :
    <br> S – the source type : 타입 변환 전 타입 
    <br> T– the target type : 타입 변환 후 타입
   
    public interface Converter<S, T>
    
    ```java
    @Slf4j
    public class IntegerToStringConverter implements Converter<Integer, String> {
        @Override
        public String convert(Integer source) {
            log.info("convert source={}", source);
            return String.valueOf(source);
        }
    }
    ```
    

1. WebConfig 를 통해 컨버터를 등록한다.
    
    ```java
    @Configuration
    public class WebConfig implements WebMvcConfigurer {
    
        @Override
        public void addFormatters(FormatterRegistry registry) {
            registry.addConverter(new IntegerToStringConverter());
        }
    }
    ```
