### Port 8080 was already in use.
```
1. cmd open
2. netstat -nao
3. taskkill /f /pid 26420 
```
netstat : network statistics, 
전송 제어 프로토콜, 라우팅 테이블, 수많은 네트워크 인터페이스, 네트워크 프로토콜 통계를 위한 네트워크 연결을 보여주는 명령 줄 도구

nestat -help로 option 확인
* a : 현재다른PC와 연결(Established)되어 있거나 대기(Listening)중인 모든 포트 번호를 확인 
* r : 라우팅 테이블 확인 및 커넥션되어 있는 포트번호를 확인 
* n : 현재 다른PC와 연결되어 있는 포트번호를 확인
* e : 랜카드에서 송수한 패킷의 용량 및 종류를 확인 
* s : IP, ICMP, UDP프로토콜별의 상태 확인
* t : tcp protocol 
* u : udp protocol 
* p : 프로토콜 사용 Process ID 노출
* c : 1초 단위로 보여줌
* o : PID(프로세스ID)를 표시


taskkill : 특정 포트 죽이기
- /f : 강제 종료
- /pid [PID] : 종료할 프로세스의 PID 지정

