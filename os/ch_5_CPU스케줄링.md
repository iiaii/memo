# CPU 스케줄링

어떤 프로그램이던지 CPU burst와 IO burst가 번갈아가며 실행된다. (빈도와 시간의 차이가 있을 수 있음)


### CPU 스케줄링이 필요한 경우

1. Running -> Blocked (ex. IO 요청하는 시스템 콜)
2. Running -> Ready (ex. 할당시간 만료로 timer interrupt)
3. Blocked -> Ready (ex. IO 완료후 인터럽트)
4. Terminate


1,4에서 스케줄링은 nonpreemptive(= 강제로 빼앗지 않고 자진 반납) 다른 스케줄링은 preemptive(= 강제로 빼앗음)
