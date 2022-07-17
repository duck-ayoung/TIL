# 제목 없음

아이템 27 : 변화로부터 코드를 보호하려면 추상화를 사용하라

추상화를 통해 변화로부터 코드를 보호하는 행위가 어떤 자유를 가져오는지 살펴보겠습니다. 세 가지 실제 사례를 살표보고, 여러 추상화의 균형을 맞추는 방법에 대해 알아봅시다. 

1. 상수
    - 리터럴을 상수 프로퍼티로 변경하면 해당 값에 의미있는 이름을 붙일 수 있으며, 상수의 값을 변경해야 할 때 훨씬 쉽게 변경할 수 있다.
    - 두 번 이상 사용되는 값은 이렇게 상수로 추출하는 것이 좋다.
    
    ⇒ 장점 : 이름을 붙일 수 있다, 나중에 해당 값을 쉽게 변경할 수 있다.
    
    ```java
    const val MIN_PASSWORD_LENGTH = 7
    fun isPaswwordValid(text: String): Boolean {
    	if(text.length < MIN_PASSWORD_LENGTH) return false
    	//...
    }
    ```
    
2. 함수
    
    예시 : 애플리케이션을 개발하는 상황에서, 사용자에게 토스트 메시지를 자주 출력해야하는 상황
    
    이렇게 사용하면, 토스트를 출력하는 코드를 항상 기억해 두지 않아도 괜찮다.
    
    이후에 토스트를 출력하는 방법이 변경되어도, 확장 함수 부분만 수정하면 되므로 유지보수성이 향상된다.
    
    Toast.makeText(this, message,  Toast.LENGTH_LONG).show()
    
    ```java
    fun Context.toast(
        message: String,
        duration: Int= Toast.LENGTH_LONG
    ) {
        Toast.makeText(this, message, duration).show()
    }
    
    context.toast(message)
    ```
    
    만약 토스트가 아니라 스낵바라는 다른 형태의 방식으로 추출해야 한다면?
    
    1. 스낵바의 확장함수를 만들고
    2.  Context.toast() → Context.snakbar()  로 한꺼번에 수정한다.
    
    ⇒ 이는 좋은 방법이 아니다. 함수의 이름을 직접 바꾸는 것은 위험할 수 있다. 다른 모듈이 이 함수에 의존하고 있다면 큰 문제가 발생할 것이다.
    
    아래처럼 바꾸는게 더 좋은 방법, 메시지를 출력하는 더 추상적인 방법
    
    여기서 가장 큰 변화는 이름이다. 어떤 개발자는 그냥 레이블을 붙이는 방식의 변화이르모 이름 변경이 큰 차이가 없다고 생각하지만, 이는 컴파일러 관점에서만 그렇다.
    
    사람의 관점에서는 이름이 바뀌면 큰 변화가 일어난 것이다. 함수는 추상화를 표현하는 수단이며, 함수 시그니처는 이 함수가 어떤 추상화를 표현하고 있는지 알려준다. 따라서 의미있는 이름은 굉장히 중요하다.
    
    함수는 매우 단순한 추상화지만, 제한이 많다. 예를 들어 상태를 유지하지 않는다. 또한 함수 시그니처를 변경하면 프로그램 전체에 큰 영향을 줄 수 있다. 구현을 추상화할 수 있는 더 강력한 방법으로는 클래스가 있다.
    
    ```java
    fun Context.showMessage(
        message: String,
        duration: MessageLength = MessageLength.LONG
    ) {
        val toastDuration = when(duration) {
            SHORT -> Length.LENGTH_SHORT
            LONG -> Length.LENGTH_LONG
        }
        Toast.makeText(this, message, toastDuration).show()
    }
    
    enum class MessageLength { SHORT, LONG }
    ```
    
3. 클래스
    1. 클래스가 함수보다 더 강력한 이유는 상태를 가질 수 있으며, 많은 함수를 가질 수 있다는 점이다. 
    2. 클래스는 훨씬 더 많은 자유를 보장해준다. 
    - 의존성 주입 프레임워크를 이용하면 클래스 생성을 위임할 수도 있다.
    - mock 객체를 활용해서 테스트할 수 있다.
    
    ```java
    class MessageDisplay(val context: Context) {
        fun show(
            message: String,
            duration: MessageLength = MessageLength.LONG
        ) {
            val toastDuration = when(duration) {
                SHORT -> Length.LENGTH_SHORT
                LONG -> Length.LENGTH_LONG
            }
            Toast.makeText(context, message, toastDuration).show()
        }
    }
    enum class MessageLength { SHORT, LONG }
    
    // 사용
    val messageDisplay = MessageDisplay(context)
    messageDisplay.show("Message")
    ```
    
4. 인터페이스
    1. 라이브러리를 만드는 사람은 내부 클래스의 가시성을 제한하고, 인터페이스를 통해 이를 노출하는 코드를 많이 사용한다. 이렇게 하면 사용자가 클래스를 직접 사용하지 못하므로, 라이브러리를 만드는 사람은 인터페이스만 유지한다면, 별도의 걱정 없이 자신이 원하는 형태로 그 구현을 변경할 수 있다. 즉, 인터페이스 뒤에 객체를 숨김으로써 실질적인 구현을 추상화하고, 사용자가 추상화된 것에만 의존하게 만들 수 있는 것이다. 
    2. 인터페이스 페이킹이 클래스 모킹보다 간단하므로, 별도의 모킹 라이브러리를 사용하지 않아도 된다.
        
         val messageDisplay: MessageDisplay = TestMessageDisplay()
        
    
    ```java
    interface MessageDisplay {
    		fun show(
    	        message: String,
    	        duration: MessageLength = MessageLength.LONG
    	   ) 
    }
    
    class ToastDisplay(val context: Context) : MessageDisplay {
        fun show(
            message: String,
            duration: MessageLength = MessageLength.LONG
        ) {
            val toastDuration = when(duration) {
                SHORT -> Length.LENGTH_SHORT
                LONG -> Length.LENGTH_LONG
            }
            Toast.makeText(context, message, toastDuration).show()
        }
    }
    enum class MessageLength { SHORT, LONG }
    ```
    

추상화의 문제

- 코드를 읽는 사람이 해당 개념을 배우고, 잘 이해해야 한다.
- 너무 많은 것을 숨기면 결과를 이해하는 것 자체가 어려워 진다.
- 추상화를 이해하려면, 예제를 살펴보는 것이 좋다.

⇒ 추상화는 단순하게 중복성을 제거해서 코드를 구성하기 위한 것이 아니다. 추상화는 코드를 변경해야 할 때 도움이 된다. 따라서 추상화를 사용하는 것은 굉장히 어렵지만, 이를 배우고 이해해야 한다. 다만 추상적인 구조를 사용하면, 결과를 이해하기 어렵다. 추상활르 사용할 때의 장점과 단점을 모두 이해하고, 프로젝트 내에서 그  균형을 찾아야한다.

아이템 28  API 안전성을 확인하라

### **안정적이고 최대한 표준적인 API를 사용하자.**

- 라이브러리의 작은 변경이 이를 활용하는 다른 코드에 영향을 미친다.
- 사용자가 새로운 API를 배워야 한다.

### **API가 불안정하다면, 명확하게 알려주자.**

- 시멘틱 버저닝(Semantic Versioning)
    - MAJOR : 호환되지 않는 수준의 변경 (0인 경우는 초기 개발 전용 버전 의미)
    - MINOR : 이전 변경과 호환되는 기능 추가
    - PATCH : 간단한 버그 수정
- 어노테이션 이용하기
    - `@Experimental`: 안정적이지 않음
    
    ```java
    @Experimental(level = Experimental.Level.Warning)
    annotaion class ExperimentalNewApi
    
    @ExperimentalNewApi
    suspend fun getUsers() : List<User> {
    
    }
    ```
    
    - `@Depreated`: 더 이상 사용되지 않음 (단, 사용자가 적응할 시간을 충분히 주자.)
        - `ReplaceWith`: 직접적인 대안 제시
        
        ```java
        @Deprecated("Old stuff", ReplaceWith("test2(i)"))
        ```
