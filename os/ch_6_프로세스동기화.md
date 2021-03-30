# 프로세스 동기화

### 경합조건 (Race Condition)

메모리 주소 공간을 공유하는 CPU 프로세스가 여럿 있는 경우 Race Condition의 가능성이 있음

-> 경합 조건이 발생할 수 있는 경우

- kernel 수행 중 인터럽트 발생시
- 프로세스가 시스템 콜을 해서 kernel mode로 수행중인데 context switch가 일어나는 경우
- 멀티프로세서에서 공유 메모리 내의 kernel data

> 커널 내부에 있는 공유데이터에 접근할때 lock/unlock을 하여 동기화 보장
 
 
