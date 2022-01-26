## filed
> HTML 태그의 id, name, value 속성을 자동으로 처리해준다.
> <br> input, select, textarea 등 태그에 따라 다르게 동작한다.

### input 태그
```
 <input type="text" th:field="*{name}" class="form-control">
```
위와 같이 사용하면 아래와 같이 id, name이 th:object에 명시된 객체의 name 필드를 참고해 id와 name 값을 생성한다.
<br> 그리고 th:vale=*{name} 도 추가된다.
```
 <input type="text" id="name" name="name" class="form-control">
```

참조 : https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#inputs
