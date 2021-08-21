# Process 개념
## 1. Job vs Process
- Job : 실행할 Program 및 Data
  - Disk에 저장되어 있고, Computer System(Kernel)에 실행요청 전 상태이다.
- Process : 실행을 위해 **Kernel에 등록**되어 버린 Job이다.
  - 성능 향상을 위해 Kernerl에 의해 관리된다.
- Process는 Job이 Kernerl에 등록되어 관리하에 있는 Job이라고 할 수 있다.
- 각종 요청 및 할당 받을 수 있는 개체이다.
  - **Active Entity**, 실행 중에 자원요구, 할당 및 반납하며 Job을 진행
- PCB를 할당 받은 개체이기도 하다. (PCB는 뒤에서 설명)
## 2. 종류
- 역할에 따라 System Process, User Process로 나뉜다.
  - System Process (Kernel Process) : 모든 System Memory, Processor 명령어에 접근이 가능하고, Process 실행 순서를 제어할 수 있으며 User Process 생성 등을 하는 Process
  - User Process : 사용자의 Code를 수행하는 Process
- 병행 수행 방법에 따라 독립 Process, 협력 Process로 나뉜다.
  - 독립 Process : 다른 Process에 영향을 받지도, 주지도 않는 Process
  - 협력 Process : 위와 반대이며, One Task를 처리하기 위한 Process들이 대표적 예다.
## 3. Resource 
- Kernel 관리하에 Process에게 할당/반납 되는 수동적인 개체다.
  - **Passive Entity**
  - H/W : Processor, Memory, etc...
  - S/W : Process, signal, etc...
## 4. PCB (Process Control Block)
- OS가 Process 관리에 필요한 정보를 저장하는 데, PCB에 저장한다.
- PCB는 **Kernel 관리 영역에 저장**된다.
- Process가 **생성되면** 생성된다.
- 관리정보 : PID, **스케줄링 정보**, **Process State**, **Memory 관리 정보**, I/O 상태, **Context 저장영역**... 등
  - OS 별로 관리정보는 다르며 PCB 참조 및 갱신은 다양한 정보를 저장하므로 OS의 성능을 결정짓는 요소 중 하나이다.
# Process State
- 자원간 상호 작용에 의해 결정한다.
## 1. Created State
![image](https://user-images.githubusercontent.com/69780812/130326776-3663f7d5-9480-4769-9a77-1e54a813381a.png)
- **Job을 Kernel에 등록**하고, **PCB를 할당**한다.
- **가용 메모리 상태를 체크**하고, 두가지 상태(Ready, Suspended Ready)로 나뉘게 된다.
  - Ready : 가용 Memory 있을 시
  - Suspended Readyd : 가용 Memory가 없을 시

## 2. Ready State
![image](https://user-images.githubusercontent.com/69780812/130326803-404f027f-3946-48f3-b5cf-0fa4ab802557.png)
- Processor (CPU) 할당 대기 상태이다.
- 즉시 실행가능 상태이다.
- Dispatch, Schedule이라함은 Ready에서 Running State로 전환됐음을 의미한다.

## 3. Running State
![image](https://user-images.githubusercontent.com/69780812/130326825-b18bff6e-bb0c-40d4-a3e4-36c1b7795f43.png)
- Processor에 필요 Resource들을 모두 할당 받은 상태이다.
- Preemtion : **Ready State로 전환**, 즉 Processor Scheduling 또는 Time-Out-Slot에 의해 **Processor를 빼앗긴 상태**이다.
- Block/Sleep : **Asleep State로 전환**, 즉 I/O가 필요한 경우 I/O가 끝나길 기다리는 상태이다.
  - Process 실행 중 입출력이 필요한 경우, 이 상태로 들어간다.

## 4. Blocked/Asleep State
![image](https://user-images.githubusercontent.com/69780812/130326856-e843d7f2-0214-4bb1-b1ad-03d12c5ff028.png)
- Processor외 다른 자원들을 기다리는 상태이다.
  - Processor도 할당이 안된 상태이다.
- 필요 자원이 할당되면, **Running State가 바로 되지 않고, Ready State로 전환**되어 Process Scheduling에 의해 Processor를 할당받을 때까지 대기한다.
  - Wake-Up

## 5. Suspended State
![image](https://user-images.githubusercontent.com/69780812/130326888-93796056-fcf3-4cca-aa2b-d3d92b47876d.png)
- **Memory** 할당을 받지 못하거나 뺏긴 상태이다.
  - Processor 아님 !
- Memory를 뺏길 때 **Memory Image를 Swap Device라는 공간에 저장**한다.
  - Memory Image : 현재 Memory State를 찍어 놓았다는 개념이다.
- Ready State에서 Memory를 빼앗기면 ? Suspended Ready State
- Asleep State에서 Memory를 빼앗기면 ? Suspended Blocked State
  - I/O를 받으면 Wake Up되어 Suspended Ready로 전환된다.
  - I/O를 받기 전에 Memory를 다시 받으면 Asleep State로 전환된다.
- Swap Device에 저장되는 것을 **Swap-in**이라하고, 복구되는 것을 **Swap-out**이라한다.

## 6. Terminated/Zombie State
![image](https://user-images.githubusercontent.com/69780812/130326972-d36c0acc-95c4-4d20-bd18-069b1919a325.png)
- Process 수행이 끝난 상태이다.
- **모든 자원을 반납**후, Kernel내 **일부 PCB 정보만 남아있는 상태**이다.
  - 이 상태의 Process는 Kernel PCB 정보 수집 후 삭제 된다.
- Process는 이 State에 들렀다가 삭제된다.

## 7. Process 관리 자료 구조
![image](https://user-images.githubusercontent.com/69780812/130327046-d3d7cb0b-63e3-49a0-a2c1-66787fe9a9fc.png)
- Ready State의 Process들은 Ready Queue 자료 구조에 저장되고, Process Scheduler 에 의해 선택된다.
- Asleep State의 Process들은 자원에 따라 각각 관리 된다.
  - 여기서 자원은 I/O값이 될 수 있고, Device의 I/O정보가 될 수 있다.

## 8. Summary Table
![image](https://user-images.githubusercontent.com/69780812/130327032-49788826-f89e-47b0-a029-dab437fb8064.png)

# Interrupt
- Unexpected + External Event
- Interrupt Evnets : I/O, Clock, Console, Program Check, Machine Check, Inter-Process(다른 Process가 Interrupt한 것), System Call 등
## Interrupt 처리 과정
![image](https://user-images.githubusercontent.com/69780812/130327507-b94e28f6-66c9-4d02-8912-56f8953886f9.png)
- Interrupt 발생 -> Process 중단 -> Interrupt Handling
  - Interrupt Handling : Interrupt 발생 장소 및 원인을 파악 후 Interrupt Service할 것인지 결정
  - Interrupt Service : Interrupt Service를 하기로 결정 후 Interrupt Service Routine 호출
```
1. Interrupt 발생 시, 해당 Process의 Processor할당을 해제한다.(Process 중단)
2. Interrupt Handler가 Interrupt Handling(Where, Why)를 수행한다.(Interrupt Handling)
3. Interrupt Handler가 해당 Interrupt의 Service Routine을 찾아 신호를 보내 Interrupt를 Service한다.(Interrupt Service)
```
## detailed
![image](https://user-images.githubusercontent.com/69780812/130327480-6eb20105-eb97-4b10-b72b-d8d2b92c858f.png)
- Interrupt Service Routine도 하나의 Process이다. 그래서 Process가 나갈 때, ISR(Interrupt Service Routine)이 진행된다.
  - Process는 나갈 때, 자신의 PCB에 Context saving을 수행한다. 
- Interrupt Service Routine에 해당하는 Process가 나가고, 나갔던 Process가 들어오는게 아니라, Ready State Queue에 있던 다른 Process가 들어올 수 있다.
  - 이 때, 다른 Process는 자신의 PCB를 통해 Context Restoring을 수행한다.

# Context Switching
- Context : Process와 관련된 정보 집합을 의미한다.
  - Process Runnig 시, MainMemory에 저장되어 있던 Data를 Register에 올리게된다.
  - 이 올라간 Register에 저장된 value들을 이 Process의 **CPU Register Context**라고한다.
  - 또한, Main Memory에 올라가져 있던 PCB, Code, Data는 **Memory Context**라고한다.
## 1. Context Saving
- Process가 시간이 지나 Processor 소유권을 잃을 때, CPU안에 있던 내용인 CPU Register Context를 Memory의 PCB에 저장하게된다. 이 작업을 Context Saving이라한다. 

## 2. Context Restoring
- CPU Register Context를 Process로 복구하는 작업을 의미한다.

## 3. Context Switching
- Context Saving + Context Restoring
  - Kernel의 개입으로 이뤄진다.
  - 현재 프로세스가 Context Saving되고, 다음 프로세스가 Context Restoring이 되는 상황이다.

## 4. Context Switching Overhead
- Context Switching 에 소요 되는 비용
- OS마다 다르며 성능에 큰 영향을 준다.
