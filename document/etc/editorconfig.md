### .editorconfig 
> EditorConfig는 다수의 개발자들이 각자 다른 에디터나 IDE를 사용해도 동일한 코딩 스타일을 유지하도록 도와주는 파일<br>
프로젝트의 root 폴더에 생성하면 그 아래에 있는 모든 파일에 적용된다.<br>
특정 폴더에서만 지정하고 싶다면 해당 폴더에 생성

#### 예시

```
root = true // root = true를 설정해야 더 상위파일로 찾아가지 않는다

// [*] : 모든 파일에 적용한다는 의미
[*]
charset = utf-8
end_of_line = lf // lf(line feed,\n):커서를 다음 줄로 이동
indent_size = 4 //들여쓰기 스페이스로 4칸
indent_style = space
trim_trailing_whitespace = true //true일 경우 문자 앞의 공백을 제거
insert_final_newline = true //true 일경우 파일 저장할 때 줄바꿈 처리
max_line_length = 120
tab_width = 4

[*.{kt,kts}] // {} : 골호안의 일치하는 파일 혹은 폴더에 적용
```
