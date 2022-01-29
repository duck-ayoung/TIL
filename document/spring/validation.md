웹 서버에서 전달 받은 요청 파라미터를 검증하는 방법
1. BindingResult : 스프링이 제공하는 검증 오류 처리 방법
<br> Controller에서 @ModelAttribute XX xx 다음 파라미터로 BindingResult bindingResult로 받으면
ModelAttribute로 받은 객체가 바인딩된 결과가 해당 파라미터로 넘어온다.
binding중 타입 오류가 난 경우 BindingResult를 파라미터로 받을 경우 오류 페이지로 넘어가지 않고 오류정보가 해당 파라미터에 저장된다.
<br> 또한 null체크나 숫자 범위 체크등의 검증이 필요한 경우 bindingResult.addError로 에러를 저장하여 넘길 수 있다.

- 넘어간 페이지에서 타임리프를 이용해서 에러 처리를 할 수 있다.
1. #fields를 이용해 bindingResult가 제공하는 검증 오류에 접근
2. th:errors를 이용해 에러가 발생하는 경우의 에러처리를 편리하게 도와준다.
