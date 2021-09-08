# Why do the Process Scheduling

- 우리 System은 Multi-programming 환경이다.
  - 여러개의 Process로 시스템이 돌아 가고있는 환경
  - 만약 CPU가 1개라면 ? -> 자원을 Process가 나눠서 써야한다.
  - 자원(CPU 등)을 할당할 Process를 선택할 필요가 있으므로 **Scheduiling**이 중요하다.

## Resource ManageMent in the Multi-Programming
1. Time Sharing 관리
- 하나의 자원 -> CPU의 경우 여러 Thread들이 번갈아가며 사용한다.
  - 사용 시간을 제한하고 사용
2. Space Shring 관리
- 하나의 자원을 분할하여 동시에 사용한다.

# Scheduling Purpose
## 1. System Performance 향상
- Performance(성능)이라는 단어는 모호하다.
- 따라서, Performance를 추정할 수 있는 정의를 해보자. (Interactive System, Real-Time System)

> Response time : 작업을 요청하면 응답받을 때 까지의 시간
  Throughput : 단위 시간동안 완료된 작업 수 (Batch System)
  Resource Utilization : 주어진 시간 Tc 동안 자원이 활용된 시간 Tr (비싼 장비) Tr / Tc 로 수치화한다.

- 목적에 맞는 지표를 고려해서 스케줄링 기법을 선택한다.
- 위의 Performance 지표 외에도 Fairness, 실행대기 방지, Predictablity(적절한 시간내 응답 보장여부) 등이 있다.

![image](https://user-images.githubusercontent.com/69780812/131222079-58a3c8a7-2f4d-43c5-acd9-d982f63f38ab.png)
- Wating Time(WT), Response Time, Turn-around Time(TT), Burst Time (BT)에 대한 정의 이므로 숙지하자.

# Scheduling Criteria
## 1. 스케줄링 기법이 고려하는 항목들
- System 특성 : Batch System VS Interactive System => 시스템은 효율적인 일처리를 위해 일처리 목적이 다를 수 있다.
- Process Urgency : Hard or Soft Real-Time VS Non-Real-Time System
- Process Priority

![image](https://user-images.githubusercontent.com/69780812/131222418-f332ed73-b3e1-4e81-a6a9-eb5278ab4f60.png)
- ***Process의 특성*** : I/O bounded VS Compute bounded
  - Process는 I/O Wating + CPU Burst 하는 시간으로 실행시간을 보낸다.
  - Compute bounded : CPU Burst Time이 많은 경우
  - I/O bounded : I/O Waiting Time이 많은 경우
  - Burst Time은 Scheduling의 중요한 Criteria이다.

# Scheduling Level
## 발생빈도 및 할당 자원에 따른 구분
1. Long-Term Scheduling (적게 발생)

![image](https://user-images.githubusercontent.com/69780812/131222445-7b20b9cd-8dd8-43de-8a73-282bfabd0afa.png)
- Job Scheduling : Job이 커널에 등록되면 프로세스가되어 Created 상태가 된다. 이 전에 Job이 대기하고 있는 queue에서 어떤 Job을 프로세스로 만들지에 대한 스케줄링
  - Multi-Programming의 Dgree(정도)를 조절한다.
    - System내 Process 수를 조절한다는 뜻이다. (Performance에 영향을 미친다.)
  - I/O, Compute - bounded Process들을 잘 섞어 선택해야된다. (효율성을 위해)
    - CPU VS I/O bounded 둘중 하나에 편중된 Job은 비효율적이다.
    - I/O만 하는 Process, CPU만 하는 Process 갖다두면 CPU-bounded 만 자원점유하고, I/O-bounded는 거의 대기만하다 끝날 수 있을 것 같다. 
- Time Sharing System에서는 모든 작업을 System에 등록하고 각 Task에 대한 시간이 주어지므로 Long Term Scheduling은 덜 중요한 편이다.

2. Mid-Term Scheduling (가끔 발생)

![image](https://user-images.githubusercontent.com/69780812/131222464-293a2650-3fad-414d-a6b7-760c24e40e75.png)
- Memeory Allocation Dicision
  - Swapping (Swap-in/out)

3. Short-Term Scheduling (자주 발생)

![image](https://user-images.githubusercontent.com/69780812/131222510-3cbe88d8-5275-4fcb-9b10-1712a9c6689b.png)
- Process Scheduling
  - 가장 빈번하게 발생한다. 따라서 매우 빨라야한다.
    - Interrupt, I/O, ... etc
    - 느리면 ? CPU 사용 시간이 100ms인데 110ms가 걸렸다 ? 10m 더필요.. 스케줄링하는데 10/110 = 9%의 Overhead가 생긴것 이다.

![image](https://user-images.githubusercontent.com/69780812/131223109-6ba58d11-83d4-4698-9fe6-31b7b09cc202.png)
- 위 사진으로 스케줄링의 단계를 이해할 수 있다.

# Shceduling Policy
1. Preemptive Scheduling
- Preemptive는 여기에서는 CPU의 선점권을 빼앗을 수 있다는 의미이다.
- 타의에 의해 Resource를 빼앗길 수 있다.
  - Time Sharing System, Real-Time System에 적합하다. 이들은 **높은 응답성을 필요하기 때문이다.**
- **Context Switching Overhead가 크다.**
2. Non-Preemtive Scheduling
- 할당 받은 Resource를 스스로 반납할때 까지 사용하고 다음 수행하는 녀석이 주도권을 가질 수 있다.
- Context Switch Overhead가 적은 편이다.
  - 평균응답시간이 빠르지만, **우선순위 역전 현상**이 잦다.
  - 우선순위 역전은 우선순위 높은 Process 말고 우선순위가 낮은 Process를 처리하는 경우다.
3. Priority
- Process의 중요도에 따라 Resource를 할당한다.
- Static Priority
  - Process 생성 시 Priority가 결정되면 그대로 유지한다.
  - 구현이 쉽고, Overhead가 적다.
  - 하지만, System 환경 변화에 대응하기가 어려울 수 있다.
- Dynamic Priority
  - Process 상태 변화에 따라 Prioirty를 변경한다.
  - 구현이 어렵고, Prioirty 재계산 등 Overhead가 발생한다.
  - System 환경 변화에 대응하기에는 좋다.

# Scheduling Algorithm
## 1. FCFS(First-Come-First-Service)
- Non-Preemtive Scheduling
- Criteria : Ready Queue 기준 도착시간
- 장점
  - Resource를 효율적으로 사용할 수 있다. (Processor가 계속 일할 수 있기 때문)
  - Processor가 계속 일할 수 있어서 Scheduling Overhead도 적다.
  - Batch System에 적합하다.
    - Response time보다는 Performance에 초점을 두고 있으며 오는 순서대로 빠른 처리가 필요하다.
- 단점
  - Interactive System에는 적합하지 않다. (무슨일을 처리하고 있더라도 필요한 작업을 요청하면 그때 그때 빠른 응답이 필요하므로)
  - Long Mean Response time
    - I/O를 통해 어떤 응답을 처리해야하는데 오래 걸리는 작업을 수행하면 그 만큼 늦게 응답을 받는다.
  - **Convey Effect**
    - 하나의 수행시간이 긴 Process에 의해 다른 Process들이 긴 Waiting Time(WT)을 갖게 되는 현상이다.

![image](https://user-images.githubusercontent.com/69780812/131223374-6bec19df-0e26-41be-9863-5ce3ac5941e0.png)
- Convey Effect를 이해하는 Traffic Jam 이미지

![image](https://user-images.githubusercontent.com/69780812/131223404-313c3a2b-e0dd-49ab-9b9d-c30da3b754ab.png)
- FCFS Flow Image

![image](https://user-images.githubusercontent.com/69780812/131223434-00192313-33c7-4520-b929-5b69a54dff8e.png)
- FCFS 연습용
- Normalized Time (TT / BT) => 체감상 Process의 Turn-Arround Time 의 지표로 이해하면된다. 1이 이상적이고, 그 이상일 수록 불공평하다 느낀다고 보면된다.

## 2. RR(Round-Robin)
- 시간을 정하고 돌아가면서 쓰자 !
- Preemptive Scheduling
- Criteria : Ready Queue 기준 도착 시간
- 자원 사용에 대해 Time Quantum(제한시간)이 있다.
- System Parameter : **Time Quantum**
- Process는 제한시간이 지나면 자원을 반납한다. (Timer-runout)
- 장점
  - 특정 Process의 자원 독점(**Monopoly**)를 방지한다.
  - Interactive System, Time Sharing System에 적합하다.
- 단점
  - Context Switch Overhead가 크다.
- **Time Quantum**이 무한대 일 수록 FCFS이고, 0에 수렴할 수록 Processor Sharing, 즉 각 Process를 동시에 쓰는 느낌이 든다.
  - 체감 Processor 속도 = 실제 Processor 성능의 1/n
  - 하지만 Context Switching Overhead는 크다.

![image](https://user-images.githubusercontent.com/69780812/131223636-4758a898-efbf-4ef3-953b-3997caaa973f.png)
- RR Flow Image
- 시간내 끝나지 못한 Process는 다시 Ready Queue의 맨 뒤로 가게 된다.

![image](https://user-images.githubusercontent.com/69780812/131223665-256b7f88-64a4-4cd5-ad69-b871b27fc2bd.png)
- RR 연습
- First Output이 없는 경우 보통 Turn-around Time이 Respons Time이다. 
- Normalized TT를 보면 보통 균등할 것이다.

## 3. SPN(Shortest-Process-Next)
- Non-Preemtive Scheduling
  - 한번 할당 받으면 끝낸다.
- Criteria : 실행시간 (Burst Time)
  - Burst Time이 가장 작은 Process를 먼저 처리한다. (SJF : Shortest Job First 라고도 한다.)
- RR은 여전히 짧은 Burst Time의 Process에 대해서는 불공평했기 때문에 등장
- 장점
  - 평균 Wait Time이 최소화 된다.
  - System 입장에서는 빨리 처리할 Process를 빨리 처리해버리니 **Process 수가 줄어든다.**
    - **Scheduling 부하가 감소**된다.
    - Memory가 절약된다.
    - System 효율이 상승
- 단점
  - **Starvation 현상**이 발생한다.
    - Burst Time이 긴 Process는 Resource를 할당 받지 못하고 기다리는 경우다.
    - 티끌모아 태산이라고.. 짧은 애들도 전부 다 처리하고 나면 많은 시간이 걸릴 수 있다.
  - 정확한 Burst Time을 예측하기란 사실상 어렵다.
    - 예측 기법이 필요하다. -> 이 부분이 매우 어렵다.

![image](https://user-images.githubusercontent.com/69780812/131223994-487117d0-b8e0-454d-a6cd-1c768a1dfbbf.png)
- SPN 연습

## 4. SRTN(Shortest Remaining Time Netxt)
- SPN의 변형
- Preemtive Scheduling
- 장점
  - SPN의 장점을 극대화한다. (WT 최소화, Scheduling 부하 감소)
- 단점
  - Process 생성 시 총 실행 시간 예측이 필요 + 잔여 시간도 계속 추적 필요 (Overhead)
  - Context Siwtch Overhead
  - 너무 비현실적이다.

![image](https://user-images.githubusercontent.com/69780812/131224015-25e78aa2-3914-4b4d-b455-53cb21859b47.png)
- SRTN 연습

## 5. HRRN(High-Response-Ratio-Next)
- SPN의 변형
  - SPN + **Aging concepts**
  - Aging concept : Process WT를 고려하고, 기회를 제공한다.
- Non-Preemptive Scheduling
- Criteria : Response Ratio, (WT + BT) / BT (응답률)
  - 위 응답률이 클수록 먼저 처리된다.
  - 필요한 시간 대비 얼마나 기다렸는지를 의미한다.
- 장점
  - Starvation 방지 + SPN 장점
- 단점
  - 실행시간 예측이 필요한건 사실.. 비현실

![image](https://user-images.githubusercontent.com/69780812/131224048-62cc353d-1923-40f2-8a8b-5a9241da388c.png)
- HRRN 연습

---
FCFS
RR
===> Fairness에 초점

SPN
SRTN
HRRN
===> System Efficiency / Performance, But 실행시간 예측하기란 어렵다.. + Probability Overhead
---

## 6. MLQ(Multi Level Queue)
- 이전 알고리즘은 Single Ready Queue 였다.
- 작업 또는 우선순위 별 별도의 Ready Queue를 갖는다.
  - Process는 최초 배정된 Ready Queue를 벗어나지 않는다.
  - 각각 Queue는 자신만의 스케쥴링 기법을 사용한다.
- 각 Queue 사이에는 Priroity Scheduling이 들어간다.
  - Fixed-Priority Preemtive Scheduling
- 장점
  - 빠른응답시간(?)
  - 사실 때마다 다른 이야기다. 중요한 Ready Queue의 Process는 빨리 처리되겠지만 그 뒤 우선순위는 **Starvation 현상**이 발생한다.
- 단점
  - 여러 Queue 관리 등 Scheduling Overhead가 크다.
  - Starvation 현상이 발생할 수 있다.

![image](https://user-images.githubusercontent.com/69780812/131224337-561dd29c-bb43-42ac-bd71-d193c9852567.png)
- MLQ 이해 이미지


## 7. MFQ(Multi-Level Feedback Queue)
- Process의 Queue간 이동을 허용한다.
- Feedback을 통해 Priority를 조정하는 것이다.
  - 현재까지 Process 사용 정보 및 패턴을 활용
- Dynamic Priority, Preemtive Scheduling 등의 특성
- 장점
  - Process에 대한 사전 정보 (BT)없이 SPN, SRTN, HRRN효과를 볼 수 있다.
- 단점
  - 설계 및 구현이 복잡하다. Scheduling Overhead가 발생
  - Starvation 문제가 존재하긴 하다.
- 변형
  - 각 Ready Queue마다 시간 할당량을 다르게 배정
    - Process의  특성에 맞는 System 운영
  - I/O bounded Process를 상위 단계 Queue로서 Priority를 높인다.
    - Process Block 시 상위 Queue로 진입
    - System 전체 Mean Response Time이 줄어들게된다.
  - 지정된 WT 넘긴 Process들을 상위 Queue로 Priority를 높인다.
    - Aging 기법 사용
- MFQ 스케줄링은 다양한 Parameter들이 있다.

![image](https://user-images.githubusercontent.com/69780812/131224386-7d49b244-940f-4b9e-82de-22545b445f88.png)
- MFQ 이해도

---
## 공부한 Algorithms
![image](https://user-images.githubusercontent.com/69780812/131224438-ea86a14b-c05c-458d-8979-ec64e00ae47a.png)



