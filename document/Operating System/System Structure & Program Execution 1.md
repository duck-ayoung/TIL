### System Structure & Program Execution

>컴퓨터 시스템은 CPU, interrupt line, mode bit, memory controller, Memery, DMA controller, timer, device Controller, local buffer, I/O device
로 구성 되어있다.

#### System structure에서 각자의 역할 
* Register : 메모리보다 빠른 영역
* Mode bit : `CPU의 제어권이 현재 사용자 프로그램에게 있는지 운영체제에게 있는지 확인할 수 있는 장치`이다.<br> 1인 경우 사용자 프로그램, 0인 경우 운영체제에게 제어권이있다.<br> 이를 통해 보안을 해칠 수 있는 중요한 명령어는 운영체제에게 제어권이 있을때만 실행하게된다.
* Interrupt line : CPU는 명령어 실행이 끝날때마다 Interrupt line을 체크하고 들어온게 있다면 이를 처리한다.<br>
Interrupt ( Interrupt (하드웨어가 발생시킨 interrupt), Trap (소프트웨어가 발생시킨 Exception, System call)
  * Exeption : 프로그램이 오류를 범한 경우 (운영체제의 메모리를 직접 접근하려고 한다거나, 0으로 나누는 행동을 하는 등 이상한 짓을 하면 interrupt line이 자동으로 셋팅이 되고 그 프로그램은 운영체제로 제어권이 넘어가게 되고 보통은 그 프로그램을 강제 종료 시킨다.)
* Timer : `특정 프로그램이 CPU를 독점하는 것을 막기 위해`서 정해진 시간이 흐른 뒤에 운영체제에게 제어권이 넘어가도록 interrupt를 발생시킨다
* Device Controller : 해당 IO 장치를 관리하는 일종의 작은 CPU. 
                      `제어 정보를 위한 register`를 가지는데 이 register에는 CPU가 device contoller에게 시킨일이 저장되어 있다.
                      Device controller는 입출력이 끝났을 경우 interrupt로 CPU에게 해당 사실을 알림
* local buffer : 입출력시 사용하는 buffer
* DMA Controller : `직접 메모리에 접근할 수 있는 Controller`<br>
  * IO 장치에서 오는 interrupt를 처리
  * IO 장치에서 너무 자주 interrupt를 걸어 CPU가 많은 방해를 받게되는 상황을 개선하기 위해 필요한 시스템
  * 바이트 단위가아니라 block 단위로 인터럽트를 발생
  * device에서 메모리에 직접 카피하고 그런 다음에 block단위로 interrupt를 걸어 IO 작업이 끝났다는 것을 CPU에게 알림
* Memory Controller : CPU와 DMA Controller가 같은 메모리를 동시에 접근했을때 발생할 수 있는 문제들을 해결하는 시스템
```
동기식 입출력 vs 비동기식 입출력
동기식 : I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램으로 넘어가는 방식, IO가 끝날 때까지 CPU가 낭비된다.
비동기식 입출력 : I/O가 시작된 후 제어가 즉시 사용자 프로그램으로 다시 넘어간다
```
#### Program Execution
> 실행 파일은 보통 하드 디스크에 파일 형태로 저장되어 있고 그런 실행 파일을 실행 시키게 되면 메모리로 올라가서 프로세스가 된다.
> 프로그램을 실행시키면 자신만의 메모리 주소 공간이 형성되는데 stack, code, data로 이루어진다.

```
함수
사용자 정의 함수 : 자신이 프로그램에서 정의한 함수
라이브러리 함수 : 자신이 정의하지 않고 갖다 쓴 함수, 프로그램의 실행 파일에 포함되어 있다.
커널 함수 : 운영체제의 프로그램 함수, 커널 함수 호출 = 시스템 콜
```
