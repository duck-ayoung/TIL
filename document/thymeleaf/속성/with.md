## with
- with 태그로 지역 변수를 사용할 수 있다.

예시 1) 기본
```
<div th:with="firstPer=${persons[0]}">
    <span th:text="${firstPer.name}"></span>.
</div>

```


예시 2) 여러개 정의
```
div th:with="firstPer=${persons[0]},secondPer=${persons[1]}">
  <p>
    <span th:text="${firstPer.name}"></span>.
  </p>
  <p>
    <span th:text="${secondPer.name}"></span>.
  </p>
</div>
```

예시 3) 같은 속성안에서 재사용 가능
```
<div th:with="company=${user.company + ' Co.'},account=${accounts[company]}">...</div>
```

------------------------------------------------------------------------------

#### 반복문에 사용할 때는 prod를 사용할 수 있다 tr 태그안에서
```
<p>
  Today is: 
  <span th:with="df=#{date.format}" 
        th:text="${#calendars.format(today,df)}">13 February 2011</span>
</p>

```
