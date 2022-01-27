## filed
> HTML 태그의 id, name, value 속성을 자동으로 처리해준다.
> <br> input, select, textarea 등 태그에 따라 다르게 동작한다.

### input 태그 type = text
```
 <input type="text" th:field="*{name}" class="form-control">
```
위와 같이 사용하면 아래와 같이 id, name이 th:object에 명시된 객체의 name 필드를 참고해 id와 name 값을 생성한다.
<br> 그리고 th:vale=*{name} 도 추가된다.
```
 <input type="text" id="name" name="name" class="form-control">
```

### input 태그 type = checkbox

```
 <input type="checkbox" th:field="*{name}" class="form-check-input">
```

추가로 checkbox 타입인 경우에는 th:feild를 사용한 경우에 \_필드이름의 hidden 타입을 추가로 만들어준다 (*chekbox 타입 특성상 check하지 않을 경우 null 값이 넘어갈 수 있는데
이를 false로 처리해주기 위해 spring에서 사용하는 방식을 자동으로 코드 생성해줌)
<br> 또한 value에 값이 있다면 checked 여부도 알아서 추가해준다.
```
<input type="checkbox" class="form-check-input" id="name" name="name" value="true">
<input type="hidden" name="_name" value="on"/>
```

참조 : https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#inputs
