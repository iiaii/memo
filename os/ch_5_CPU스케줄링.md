# CPU 스케줄링

어떤 프로그램이던지 CPU burst와 IO burst가 번갈아가며 실행된다. (빈도와 시간의 차이가 있을 수 있음)


### CPU 스케줄링이 필요한 경우

1. Running -> Blocked (ex. IO 요청하는 시스템 콜)
2. Running -> Ready (ex. 할당시간 만료로 timer interrupt)
3. Blocked -> Ready (ex. IO 완료후 인터럽트)
4. Terminate


1,4에서 스케줄링은 nonpreemptive(= 강제로 빼앗지 않고 자진 반납 = 비선점) 2, 3에서 스케줄링은 preemptive(= 강제로 빼앗음 = 선점)


> CPU 스케줄링이 필요한 이유는 IO bound job과 CPU bound job이 서로 섞여있기 때문

### Scheduling Criteria

- CPU utilization (이용률) -> 주방장이 놀지 않고 일한 시간
- Throughput (처리량) -> 단위 시간당 손님이 몇명 왔다 갔는가
- Turnaround time (소요시간, 반환시간) -> 손님이 음식 주문 후 다 먹고 나가는 시간
- Wating time (대기시간) -> 손님이 기다린 시간
- Response time (응답시간) -> 첫 번째 음식이 나오는 시간


### 스케줄링 방법

- FCFS (FIFO)
- SJF (CPU burst 가 짧은 것 부터 실행) -> starvation 이 발현되어 아예 실현되지 않는 경우가 있음 -> aging을 통해 해결
- RR

각 프로세스는 동일한 크기의 할당 시간을 가지고 할당시간이 지나면 프로세스는 선점당하고 제일 뒤에 가서 다시 줄을 서기 때문에 응답 시간이 빠르다. 
할당시간이 커지면 FCFS, 짧아지면 컨텍스트 스위칭 비용이 커진다. (현대의 컴퓨터는 RR에 기반, CPU 사용시간이 가늠이 잘 안될때 잘 적용됨)

- 멀티 레벨 피드백 큐 (RR 만으로는 CPU burst가 긴 것들이 잘 실행되지 않기 때문에 활용된다)

> 멀티 프로세서 스케줄링도 크게 다르지 않음 
 





