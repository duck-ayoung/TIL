## Bean Validation
> Bean Validation 2.0이라는 가술 표준. Bean Validation을 구현한 기술중에 일반적으로 사용하는 구현체는 하이버네이트 Validator

## 사용법

1. gradle 의존관계 추가
```
implementation 'org.springframework.boot:spring-boot-starter-validation'
```
2. 검증 애노테이션 사용
예시) 
```
@NotBlank
private String ame;
```
