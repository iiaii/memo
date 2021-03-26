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

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행

-> CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
-> CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴

> system call이나 interrupt 발생시에 context switch가 일어나는 것은 아니다. (사용자 프로세스 -> 운영체제 는 문맥 교환이 아님)
> 시스템 콜 이후 문맥 교환 없이 user mode로 복귀하는 것 역시 수행정보 등 context 일부를 PCB에 save 해야 하지만 a 프로세스에서 b 프로세스로의 문맥 교환은 부담이 훨씬 크다. 
> (문맥교환이 일어나면 캐시 메모리 flush 함)



### 스케줄러

- Long-term scheduler (장기 스케줄러)

-> 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
-> 프로세스에 메모리를 주는 문제
-> degree of Multiprogramming을 제어
-> time sharing system에는 보통 장기 스케줄러가 없음 (무저건 ready)

- Short-term scheduler (단기 스케줄러)

-> 어떤 프로세스를 다음번에 running 시킬지 결정
-> 프로세스에 CPU를 주는 문제
-> 충분히 빨라야 함

- Medium-Term Scheduler (중기 스케줄러)

-> 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아 냄
-> 프로세스에게서 메모리에서 디스크로 쫓아냄
-> degree of Multiprogramming을 제어
