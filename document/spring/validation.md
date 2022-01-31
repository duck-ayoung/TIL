웹 서버에서 전달 받은 요청 파라미터를 검증하는 방법
1. BindingResult : 스프링이 제공하는 검증 오류 처리 방법

**BindingResult?**
```
General interface that represents binding results. 
Extends the interface for error registration capabilities, allowing for a Validator to be applied, and adds binding-specific analysis and model building.
Serves as result holder for a DataBinder, obtained via the DataBinder.getBindingResult() method. 
BindingResult implementations can also be used directly, for example to invoke a Validator on it (e.g. as part of a unit test).

binding 결과를 나태내는 일반적인 인터페이스이다.
오류 등록 기능에 대한 인터페이스를 확장하여, validator를 적용할 수 있도록 하고, 바인딩별 분석 및 모델 building을 추가한다.
DataBinder.getBindingResult() 메서드를 통해 얻은 DataBinder의 결과 홀더 역할을 한다.
BindingResult 구현은 예를들어 Validator를 호출하는데 직접 사용할 수도 있다. (예 : 단위테스트의 일부로)


Controller에서 @ModelAttribute XX xx 다음 파라미터로 BindingResult bindingResult로 받으면
ModelAttribute로 받은 객체가 바인딩된 결과가 해당 파라미터로 넘어온다.
binding중 타입 오류가 난 경우 BindingResult를 파라미터로 받을 경우 오류 페이지로 넘어가지 않고 오류정보가 해당 파라미터에 저장된다.
```

**메서드**
1. addError(ObjectError error)
```
Add a custom ObjectError or FieldError to the errors list.

ObjectError, FieldError 를 errors list에 추가
AbstractBindingResult
@Override
public void addError(ObjectError error) {
  this.errors.add(error);
}

```
  - ObjectError : Encapsulates an object error, that is, a global reason for rejecting an object. <br> 객체를 거부하는 global reseason을 캡슐화 
  ```Java
  public ObjectError(String objectName, String defaultMessage)
  public ObjectError(String objectName, @Nullable String[] codes, @Nullable Object[] arguments, @Nullable String defaultMessage)
      
  /**
  * objectName : @ModelAttribute의 이름
  * codes : 메시지 코드
  * arguments : 메시지에서 사용하는 인자
  * defaultMessage : 오류 기본 메시지
  */
  ```

  - FieldError : Encapsulates a field error, that is, a reason for rejecting a specific field value. <br> field value를 거부하는 이유를 캡슐화 
  ```Java
  public FieldError(String objectName, String field, String defaultMessage)
  public FieldError(String objectName, String field, @Nullable Object rejectedValue, boolean bindingFailure,
			@Nullable String[] codes, @Nullable Object[] arguments, @Nullable String defaultMessage) 
      
  /**
  * objectName : @ModelAttribute의 이름
  * field : 오류가 발생한 필드 이름
  * rejectedValue : 사용자가 입력한 값(거절된 값)
  * bindingFailure : 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분하는 값
  * codes : 메시지 코드
  * arguments : 메시지에서 사용하는 인자
  * defaultMessage : 오류 기본 메시지
  */
  ```
2. rejectValue(), reject()
 	- reject : Register a global error for the entire target object, using the given error description. <br> error description을 사용하여 target object에 대한 global error를 등록
	- rejectValue : Register a field error for the specified field of the current object (respecting the current nested path, if any), using the given error description. <br>error discription을 사용하여 현재 object의 지정된 필드에 대하여 필드 오류를 등록한다.


addError, rejectValue 쓸 때 예시
```Java
bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, new String[]{"required.item.itemName"}, null, null));

/**
*1. bindingResult는 어떤 객체를 binding하는지 알 고 있기 때문에 객체 생략가능
*2. propertyProcessor를 이용해서 rejectedValue도 자동으로 넣어주기 때문에 생략 가능
*3. MessageCodeResolver를 이용해서 String 배열이 아닌 "required"만 입력 가능
*/
bindingResult.rejectValue("itemName", "required");


AbstractBindingResult
@Override
public void rejectValue(@Nullable String field, String errorCode, @Nullable Object[] errorArgs,
		@Nullable String defaultMessage) {

	if (!StringUtils.hasLength(getNestedPath()) && !StringUtils.hasLength(field)) {
		// We're at the top of the nested object hierarchy,
		// so the present level is not a field but rather the top object.
		// The best we can do is register a global error here...
		reject(errorCode, errorArgs, defaultMessage);
		return;
	}

	String fixedField = fixedField(field);
	Object newVal = getActualFieldValue(fixedField);
	FieldError fe = new FieldError(getObjectName(), fixedField, newVal, false,
				resolveMessageCodes(errorCode, field), errorArgs, defaultMessage);
	addError(fe);
}

@Override
@Nullable
protected Object getActualFieldValue(String field) {
	return getPropertyAccessor().getPropertyValue(field);
}
```

5. 타입 오류 : Spring이 FiledError를 생성해서 BindingResult에 넣어줌<br>
예시) item 객체의 price 필드에 숫자가 아닌 문자를 넣었다.
```
object 'item' on field 'price':
rejected value [ㅁㄴㅇㅁ]; 
codes [typeMismatch.item.price,typeMismatch.price,typeMismatch.java.lang.Integer,typeMismatch]; 
arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [item.price,price];
arguments []; 
default message [price]]; 
default message [Failed to convert property value of type 'java.lang.String' to required type 'java.lang.Integer' for property 'price'; nested exception is java.lang.NumberFormatException: For input string: "ㅁㄴㅇㅁ"]
```

## Thymeleaf
- 넘어간 페이지에서 타임리프를 이용해서 에러 처리를 할 수 있다.
#fields를 이용해 bindingResult가 제공하는 검증 오류에 접근
th:errors를 이용해 에러가 발생하는 경우의 에러처리를 편리하게 도와준다.
