### Fatal exception

>처리되지 않은 예외나 신호로 인해 예상치 못한 종료가 발생할 때마다 발생하고 이 경우 Android 앱이 비정상 종료된다.
이때 발생하는 exeption에 대한 예외를 처리한다면 앱이 비정상 종료되는 것을 막을 수 있다.

>ex) null값이나 빈 문자열을 확인하는 등의 작업, API에 무효한 인수를 전달하는 작업, 복잡한 멀티스레드 상호작용 

##### stack trace에 표시되는 정보 : 

1. 발생한 예외 유형 (ex NullPointerExceptio)
2. 예외가 발생한 코드 섹션 (ex (MainActivity.java:27), (View.java:6134) ... )
```
    --------- beginning of crash
    AndroidRuntime: FATAL EXCEPTION: main
    Process: com.android.developer.crashsample, PID: 3686
    java.lang.NullPointerException: crash sample
    at com.android.developer.crashsample.MainActivity$1.onClick(MainActivity.java:27)
    at android.view.View.performClick(View.java:6134)
    at android.view.View$PerformClick.run(View.java:23965)
    at android.os.Handler.handleCallback(Handler.java:751)
    at android.os.Handler.dispatchMessage(Handler.java:95)
    at android.os.Looper.loop(Looper.java:156)
    at android.app.ActivityThread.main(ActivityThread.java:6440)
    at java.lang.reflect.Method.invoke(Native Method)
    at com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:240)
    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:746)
    --------- beginning of system
```
    참조</br><a>https://developer.android.com/topic/performance/vitals/crash?hl=ko
