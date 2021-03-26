# 프로세스

`Process is a program in execution`

- 프로세스의 문맥
  - CPU 수행 상태를 나타내는 하드웨어 문맥 (Program Counter, 각종 register)
  - 프로세스의 주소 공간 (code, data, stack)
  - 프로세스 관련 커널 자료구조 (PCB, Kernel stack)


> 현대의 컴퓨터는 타임쉐어링을 하기 때문에 프로세스의 문맥을 알아야 한다 (당시 실행시점을 알아야 이어서 실행할 수 있기 때문)
 

- 프로세스는 상태가 변경되며 수행된다

  - Running : CPU를 잡고 instruction을 수행중인 상태
  - Ready(in memory) : CPU를 기다리는 상태 (메모리 등 다른 조건을 모두 만족하고)
  - Blocked (wait, sleep) : CPU를 주어도 당장 instruction 수행할 수 없는 상태, Process 자신이 요청한 event가 즉시 만족되지 않아 이를 기다리는 상태 (디스크에서 file을 읽어와야 하는 경우)

> new - 프로세스가 생성중인 상태, Terminated - 수행이 끝난 상태
 

### PCB

- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
- 다음을 구조체로 유지함

(1) OS가 관리상 사용하는 정보

- Process state, Process ID
- scheduling information, priority

(2) CPU 수행 관련 하드웨어 값

- Program counter, registers

(3) 메모리 관련

- Code, data, stack 의 위치 정보

(4) 파일 관련

- Open file descriptors ...


### 문맥 교환 (context switching)

- CPU를 한 
