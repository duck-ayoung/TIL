# Defendency Injection

[https://developer.android.com/training/dependency-injection](https://developer.android.com/training/dependency-injection)

```kotlin
class Car {

    private val engine = Engine()

    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val car = Car()
    car.start()
}
```

![1-car-engine-no-di](https://user-images.githubusercontent.com/80760858/157397325-ff6f92d7-59e8-4ca9-914a-70d1d64b87c8.png)

⇒ 이것은 Car class가 Engine을 직접 생성하고 있기 때문에 DI가 아니다.  이렇게 구현할 경우 문제점

1. Car와 Engine의 높은 결합도 - Car의 인스턴스는 한가지 유형의 Engine을 사용하므로 서브클래스나 대체 구현을 쉽게 사용할 수 없다. 만일 Car class가 Engine을 직접 생성했다면, Gas와 Electric엔진 타입을 사용할때는 Car를 재사용하는 대신에 각가 두가지 타입의 Car 클래스를 만들어야한다. 
    
    ```kotlin
    //예시
    class Car1 {
    
        private val engine = Gas()
    
        fun start() {
            engine.start()
        }
    }
    
    class Car2 {
    
        private val engine = Electric()
    
        fun start() {
            engine.start()
        }
    }
    ```
    
    - SubClass?
        
        객체지향 세계에서 상속은 크게 두가지로 존재
        
        SubClassing - 이미 구현이 되어 있는 클래스를 구현 상속할때 사용되는 말
        
        SubTyping - interface implementation
        
2. 강한 의존성은 테스팅을 더 어렵게 만든다. Car는 Engin의 실제 인스턴스를 사용므로 테스트 더블을 사용하여 다양한 테스트 케이스에 대해 Engine을 수정할 수 없다.
    - Test double
        - 테스트를 진행하기 어려운 경우 이를 대신해 테스트를 진행 할 수 있도록 만들어주는 객체

```kotlin
class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val engine = Engine()
    val car = Car(engine)
    car.start()
}
```

![1-car-engine-di](https://user-images.githubusercontent.com/80760858/157397418-facf5478-bf2f-4fcd-b102-2a3a548f45df.png)


⇒ DI 사용시의 코드. Car class 초기화시에 Engine을 직접 생성하는 대신 생성자 매개변수로 Engine 을 받았다. main 함수에서 Car를 사용한다. Car는 Engine에 의존하므로 앱은 Engine을 생성한 후 이를 사용하여 Car를 생성한다. 

DI 기반 접근 방식의 장점

1. Car 재사용 가능 - Engine의 다양한 구현을 Car에 전달 할 수 있다.
2. 테스트 편의성 -  test double을 전달하여 다양한 시나리오를 테스트할 수 있다. 예를 들면 FakeEngine이라는 Engine의 테스트 더블을 만들어 테스트에 맞게 구성할 수 있다.

### 안드로이드에서 DI를 하는 주요한 두가지 방법

- 생성자 주입 - 위의 예시에서 사용한 방법, 클래스의 생성자에 전달
- 필드 주입 - activity와 fragment와 같은 특정 안드로이드 프레임워크 클래스는 시스템에서 생성하므로, 생성자 주입을 사용할 수 없다. 필드 주입을 사용하면 클래스가 생성되고 난 후에 종속(dependecies) 항목이 인스턴스화된다.
    
    ```kotlin
    class Car {
        lateinit var engine: Engine
    
        fun start() {
            engine.start()
        }
    }
    
    fun main(args: Array) {
        val car = Car()
        car.engine = Engine()
        car.start()
    }
    ```
    

### **Automated dependency injection**

- 위의 예제에서는 라이브러리 없이 다양한 클래스의 종속 항목을 직접 생성하고 제공하고 관리했다. 이것을 *dependency injection by hand* 또는 *manual dependency injection* 라고 부른다. Car의 예시에서는 오직 하나의 dependency만 있었지만 종속 항목과 클래스가 많아지면 수동으로 종속 항목을 삽입하는 작업이 더 지루해질 수 있다. 또한 수동 종속 항목 주입은 다음과 같은 문제도 있다.
    1. 대규모 앱의 경우, 모든 dependencie를 올바르게 연결하려면 많은 보일러플레이트 코드가 필요할 수 있다. 다중 레이어 아키텍처에서는 최상위 레이어를 만들기 위해서 하위 레이어의 dependencies를 모두 제공해야한다. 
- dependencies를 생성하고 제공하는 과정을 자동화하여 이 문제를 해결하는 라이브러리가 있다.
    - runtime 시에 dependencies를 연결하는 Reflection-based 솔루션
    - 컴파일 타임에 dependencies를 연결하고 생성하는 정적 솔루션 -  Dagger

### **Alternatives to dependency injection**

- DI의 대안은 서비스 로케이터를 사용하는 것입니다. 또한 서비스 로케이터 디자인 패턴은 종속성으로부터 클래스의 decoupling을 향상시킨다. dependencies를 생성하고 저장한 후 필요에 따라 이러한 dependencies를 제공하는 서비스 로케이터라는 클래스를 생성한다.

```kotlin
object ServiceLocator {
    fun getEngine(): Engine = Engine()
}

class Car {
    private val engine = ServiceLocator.getEngine()

    fun start() {
        engine.start()
    }
}

fun main(args: Array) {
    val car = Car()
    car.start()
}
```

- 서비스 로케이터 패턴은 elements가 소비되는 방식에서 DI와 다르다. 서비스 로케이터 패턴을 통해 클래스는 주입할 객체를 제어하고 요청한다.
    
    DI와의 차이  
    
    1. 서비스 로케이터에 필요한 dependencies 모음은 모든 테스트가 같은 global 서비스 로케이터와 상호작용해야하므로 코드를 테스트하기 어렵다.
    2. 전체 앱의 lifetime이 아닌 다른 기간으로 범위를 지정하려는 경우 객체의 lifetime을 관리하는 것이 더 어렵다.

### **Use Hilt in your Android app**

- Hilt는 안드로이드에서 DI를 위한 Jetpacks의 권장 라이브러리이다. Hilt는 프로젝트의 모든 Android 클래스에 컨테이너를 제공하고 수명 주기를 자동으로 관리함으로써 애플리케이션에서 DI를 사용하는 표준 방법을 정의한다.
- Hilt는 Dagger가 제공하는 컴파일 타임 정확성, 런타임 서능, 확장성 및 Android 스튜디오 지원의 이점을 누리기 위해 인기 있는 DI 라이브러리 Dagger를 기반으로 빌드되었다.

### 결론

DI는 다음과 같은 이점을 제공한다.

1. 클래스 재사용과 의존성 decoupling 
2. 리펙토링 편의성
3. 테스트 편의성

참조 : 

[https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/](https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/)
