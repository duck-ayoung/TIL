## Companion Object

#### 1. 팩토리 메소드와 정적 멤버가 들어갈 장소
>코틀린 클래스 안에는 정적인 멤버가 없고 이 정적인 메소드, 멤버를 대신하는 "최상위 함수"는 클래스 내의 private 멤버에 접근할 수 없는 한계가 존재하기 때문에 이런 상황에 companion이라는 키워드를 이용하여 클래스의 companion object를 생성한다. 
<pre><code>
  Class PhoneBook private(val info: PhonNumberInfo){
    companion object {
      fun newNaverPhoneBook(naverId : String) = PhoneBook(getPhonNumberInfo())
    }
   }
</code></pre>

#### 2. companion object의 프로퍼티나 메소드에 접근하려면 companion object가 정의된 클래스 이름을 사용한다.</br>
   아래의 코드에서 print를 호출하려면: PhoneNumber.print() 
   <pre><code>
   Class PhoneNumber {
    companion object {
      fun print() {
          ...
      }
   }
   </code></pre>
