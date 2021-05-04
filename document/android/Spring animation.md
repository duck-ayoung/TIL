### Spring animation

> 스프링 기반 애니메이션에서 SpringForce 클래스를 사용하면 스프링의 강성, 감쇠 비율 및 최종 위치를 맞춤 설정할 수  있다.

1. 지원 라이브러리 추가 (build.gradle)
```
dependencies {
          implementation 'androidx.dynamicanimation:dynamicanimation:1.0.0'
}
```

2. 예제 코드
MainActivity
```
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val imgView = findViewById<ImageView>(R.id.imageView)
        val imgViewSpringAnimation = SpringAnimation(imgView, DynamicAnimation.TRANSLATION_Y, 0f).apply {
            spring.stiffness = SpringForce.STIFFNESS_LOW
            spring.dampingRatio = SpringForce.DAMPING_RATIO_HIGH_BOUNCY
        }

        imgViewSpringAnimation.animateToFinalPosition(200f)

        imgViewSpringAnimation.start()
    }
}
```
activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/ic_launcher_foreground" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
