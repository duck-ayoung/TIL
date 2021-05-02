### System Structure & Program Execution 1

>컴퓨터 시스템은 CPU, interrupt line, mode bit, memory controller, Memery, DMA controller, timer, device Controller, local buffer, I/O device
로 구성 되어있다.

* Register : 메모리보다 빠른 영역
* Mode bit : `CPU의 제어권이 현재 사용자 프로그램에게 있는지 운영체제에게 있는지 확인할 수 있는 장치`이다.<br> 1인 경우 사용자 프로그램, 0인 경우 운영체제에게 제어권이있다.<br> 이를 통해 보안을 해칠 수 있는 중요한 명령어는 운영체제에게 제어권이 있을때만 실행하게된다.
* Interrupt line : CPU는 명령어 실행이 끝날때마다 Interrupt line을 체크하고 들어온게 있다면 이를 처리한다.<br>
  * Interrupt ( Interrupt (하드웨어가 발생시킨 interrupt), Trap (소프트웨어가 발생시킨 Exception, System call)
* Timer : `특정 프로그램이 CPU를 독점하는 것을 막기 위해`서 정해진 시간이 흐른 뒤에 운영체제에게 제어권이 넘어가도록 interrupt를 발생시킨다
* Device Controller : 해당 IO 장치를 관리하는 일종의 작은 CPU. 
                      `제어 정보를 위한 register`를 가지는데 이 register에는 CPU가 device contoller에게 시킨일이 저장되어 있다.
                      Device controller는 입출력이 끝났을 경우 interrupt로 CPU에게 해당 사실을 알림
* local buffer : 입출력시 사용하는 buffer
* DMA Controller : `직접 메모리에 접근할 수 있는 Controller`. IO 장치에서 오는 interrupt를 처리한다.
                  IO 장치에서 너무 자주 interrupt를 걸어 CPU가 많은 방해를 받게되는 상황을 개선하기 위해 필요한 시스템
* Memory Controller : CPU와 DMA Controller가 같은 메모리를 동시에 접근했을때 발생할 수 있는 문제들을 해결하는 시스템
