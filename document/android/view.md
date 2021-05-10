### View


> 사용자가 보고 상호작용 할 수 있는 것을 그린다. <br>
> 일반적으로 위젯이라고하고 viewGroup 객체는 일반적으로 레이아웃(View를 포함하고 배치하는 역할)이라고한다.

![view 계층구조](https://developer.android.com/images/viewgroup_2x.png)
##### * Button은 TextView를 상속받아 만들어졌기 때문에 TextView의 특성을 활용할 수 있다.<br>

* View는 화면의 일정 영역을 차지하기 때문에 **반드시 크기 속성**을 가지고 있어야 한다.
* View의 위치는 getLeft(), getTop() 메서드를 통해 검색 가능하다. (Y는 상위 요소에 상대적인 뷰의 위치를 반환한다)
* View의 크기를 확인하려면 getMeasuredWidth(), getMeasuredHeight()
* View는 Padding은 정의할 수 있지만(setPadding()) 여백은 정의할 수 없다.
* getWidth() getHeight() 너비높이?
* Button은 TextView를 상속받아 만들어졌기 때문에 TextView의 특성을 활용할 수 있다.
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"    //크기 속성 필수
              android:layout_height="match_parent"   //android: 안드로이드 기본 API에서 정의한 속성
              android:orientation="vertical" >
```

참조<br>
<a>https://developer.android.com/guide/topics/ui/declaring-layout
<a>https://developer.android.com/reference/android/widget/Button
