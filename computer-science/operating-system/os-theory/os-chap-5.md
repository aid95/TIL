# CPU 스케쥴링

## CPU 스케쥴링

CPU 스케쥴링은 멀티 프로그래밍 운영체제의 기본이며, 멀티 프로그래밍의 항상 어떠한 프로세스를 실행할 수 있고, CPU 활용을 극대화하기 위한 목적이다.

CPU 스케쥴링의 기본 컨셉은 우리가 사용하는 대부분의 프로세스는 CPU burst보다 I/O burst인 시간이 더 많고, 따라서 I/O burst인 시간에 CPU burst인 다른 프로세스를 수행하게 되면 성능의 올릴 수 있다는 것이다. 따라서 CPU 스케쥴링은 Reday큐에 있는 다음 CPU가 필요한 프로세스를 선정해야하는데 매우 간단하게 FIFO queue와 Priority queue를 생각해볼 수 있다.

- Non-preemptive scheduling
  - 현재 CPU를 선점 중인 프로세스가 자발적으로 종료 또는 Wating 큐로 스위칭하기 전까지 내버려둔다.
- Preemptive scheduling
  - 현재 CPU를 선점 중인 프로세스를 강제로 쫓아낼 수 있다.

프로세스의 라이프 사이클에서 CPU 스케쥴링은 다음과 같은 의사 결정을 할 수 있다.

1. 실행 상태에서 대기 상태로 전환될 때
2. 실행 상태에서 준비 상태로 전환될 때
3. 대기 상태에서 준비 상태로 전환될 때
4. 프로세스가 종료될 때

여기서 1번과 4번은 선택권이 없이 Non-preemptive해야하고, 2번과 3번은 Preemptive와 Non-preemptive중 선택해야한다.

### Dispatcher

Dispatcher는 CPU core를 제어하는 모듈로 CPU 스케쥴러로 부터 프로세스를 선택해 다음과 같은 작업을 수행한다.

- 하나의 프로세스에서 다른 프로세서의 문맥 교환(context switching)
- User mode로 전환
- 사용자 프로그램의 재개를 위한 올바른 위치로 이동

![](https://i.imgur.com/A54k7Ra.png)

앞서 공부했던 것과 관련하여 우리는 프로세스가 다른 프로세스로 전환되는 문맥 교환시 현재 프로세스의 재개를 위한 PCB를 저장과 불러오는 작업이 필요하단 걸 알고 있다. 위 사진에서 앞서 PCB를 저장하고 불러오는 시간이 dispatch latecy고 만약 dispatcher가 너무 빠르게 이뤄진다면 모든 프로세스는 수행하기 전 문맥 교환만 이뤄져 시스템이 다운될 수 있다.

이렇듯 스케쥴러의 목표는

- CPU의 효율성 극대화
- 일정 시간안에 최대한 많은 프로세스의 완료
- 요청에서 부터 종료까지의 최소한의 수행 시간
- 어떤 프로세스가 Ready queue에서 대기하는 시간을 최소화
  - Ready queue의 대기 시간의 합을 최소화
  - 가장 중요한 목표이며, 이 목표가 이뤄지면 부과적으로 다른 목표도 이뤄질 수 있다.
- 응답 시간 최소화

이렇듯 CPU 스케쥴리는 Ready queue에서 어떤 프로세스를 선택할 것인가에 대해서 고민해야 한다.

- FCFS : 먼저 들어온 프로세스를 선정한다.
  - 구현이 매우 간단
  - 수행할 프로세스의 순서에 의해 매우 비효율적으로 처리될 수 있음
    - 매우 긴 CPU burst를 가진 프로세스 먼저 처리되면 짧은 CPU burst를 가진 프로세스는 오랜시간 기다려야 함 (Convoy effect)
  - Non-preemptive
- SJF : 짧은 수행 시간을 가지는 프로세스를 선정한다.
  - 다음 CPU burst가 짧은 프로세스를 선정해야 함
  - FCFS의 Convoy effect를 해결함
  - 만약 모든 CPU burst가 같다면 FCFS와 같음
- RR : 시분할하여 프로세스를 선정한다.
- Priority-based : 우선순위를 부여하여 선정한다.
- MLQ : 상황에 따라 적절한 스케쥴링 알고리즘을 사용한다.
- MLFQ : 피드백을 줘 유동적으로 현재 상황에서 적절한 스케쥴리을 선택한다.