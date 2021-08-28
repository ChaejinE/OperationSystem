# Process와 Thread
![image](https://user-images.githubusercontent.com/69780812/131218578-199b99e0-ad36-490f-accc-a36387b1ec42.png)
- Process는 자원을 할당받고, Process가 자원을 활용해서 제어한다.
- 여기서 Control(제어)하는 녀석이 Thread이다.
  - 자원을 활용 및 제어하는 녀석.
- Thread는 여러개 있을 수 있다. => Control을 여러개할 수 있다.

![image](https://user-images.githubusercontent.com/69780812/131218760-acb58590-06a5-4ada-9d8a-576a1f94c8cc.png)
- Process안에 자원을 제어하는 Thread가 여러개있다.

![image](https://user-images.githubusercontent.com/69780812/131218806-c81a64bd-9492-414b-8eb9-0088f200ba05.png)
- Thread는 제어 정보를 갖는다.
  - 제어 정보 : Stack Pointer(SP), Program Counter(PC), 다양한 상태정보 등
  - local data : 지역 범위 변수 데이터 (for { int a; }의 a)
  - stack : local data를 제어하기위해 stack 영역에 쌓는다.
- Resource는 CODE, global data, Heap 영역있다.
  - CODE : 프로그램을 수행할 명령, Porgram Counter가 이 부분을 가리키고있다.
  - Global Data : 전역 변수 저장 영역
  - Heap : Malloc 등의 명령어를 사용하면 이 영역에 저장된다.
  - Resource는 Process에 할당된 자원들이다. Thread는 이러한 Resource를 **공유한다.**
- 각 Thread가 가진 정보로 각 작업을 처리한다. **하지만, 하나의 프로세스가 할당받은 자원은 공유한다.**

![image](https://user-images.githubusercontent.com/69780812/131219213-433c9c92-4386-4696-9fb8-2f7746f15f9f.png)
- 위 그림은 프로세스의 메모리 공간을 이해하는 데 도움이 된다.
- Thread들은 동일한 Resource 공간을 공유하고 있다.

# Thread
- Light Weight Process라고도 부른다. (LWP)
  - Process : Resource + Control
  - Thread : Control
- Processor(CPU)의 활용 기본 단위이다.
  - Process내의 각 Thread가 CPU를 할당받고 주어진 수행명령을 통해 제어를 하기 때문에 그런 것 같다.
## 1. 구성요소
- Thread-ID
- Register set (PC, SP 등)
- Stack (local data)
- 제어 요소의 CODE, Data 자원은 다른 Thread들과 공유한다.
## 2. Thread의 장점
- Responsiveness
  - 일부 Thread가 지연되더라도 다른 Thread가 작업을 계속 처리가 가능하다.
  - FPS Game시 Display, Mouse, Speaker Sound등의 작업을 처리해야하는데, 알다 시피 Process에 I/O가 발생하면 block 상태가 되며 다시 ready 상태에서 running으로 올라가야하는 상황이 발생한다.
  - 스레드는 위처럼하지 않고, 각 스레드가 Display, Mouse, Speaker를 맡아 I/O가 발생하더라도 Process가 block 상태로 가지 않고, running 상태에서 다른 Thread는 자기 일을 처리할 수 있다. User는 I/O외의 작업에 대한 응답을 빠르게 받을 수 있다.
- Resource sharing
  - 시스템의 효율성을 증가시킨다.
  - 하나의 Resource를 가지고 Process가 번갈아 작업을 한다면 Context Switching같은 비싼 비용이 발생한다.
  - Thread는 Process와 같이 비싼 비용의 Context Swiching 없이 자원을 공유하며 일을 처리해 나갈 수 있다. 
  - Context Switching이 일어나지 않는 다는 것은 kernel의 개입이 없다는 것이므로 Context Switching으로 인한 시스템의 Overhead가 줄어들 수 있다는 것이다.
- Economy
  - Process 생성, Context Switch에 비해 효율적이기 때문이다.
- Multi-Processor 활용
  - 여러 스레드를 생성할 수 있으므로 여러 Core가 있는 CPU 환경이면 **병렬 처리를 통해** 각 스레드가 각 Core를 할당받아 사용하지 않는 Processor 자원없이 시스템의 Performance(성능)을 향상시킬 수 있다.

## 3. Thread 구현
### 1. User 수준의 Thread (n:1 model)
![image](https://user-images.githubusercontent.com/69780812/131220556-de1a4ba6-9072-418b-a683-0766f913fe9b.png)
- Thread Library로 구현된다.
  - Library를 통해 Thread 가 여러개 있는 것처럼 한다.
- POSIX, Win32 Thread, Java API 등
- Library가 Thread를 생성 및 스케줄링 한다.
- 장점
  - 커널이 Thread의 존재를 모른다.
  - Kernel의 관리 및 개입을 받지 않으므로 생성 및 관리의 부하가 줄어들어 유연한 관리가 가능하다.
  - Library만 있으면 Multi Thread로 사용이 가능하므로 Probability(이식성)가 높다.
- 단점
  - Kernel은 Process 단위로 자원을 할당한다.
  - 이는 하나 이상의 Thread가 I/O로 Block이되면, 모든 Thread도 대기상태로 넘어가버린다.
  - Kernel은 이 Thread들의 존재를 모르기 때문에 Single Thread로 보고, Asleep 상태로 전환 시켜버리기 때문이다.

### 2. Kernel Thread (1:1 model)
![image](https://user-images.githubusercontent.com/69780812/131221090-5d9d0fb7-2d82-469c-806d-0d0a814451d8.png)
- OS가 Thread를 직접 관리한다.
  - Kernel 영역에서 Thread의 생성과 관리를 수행한다. Context Switching으로 Overhead가 생긴다.
  - Process 수준의 Context Switching은 아니지만 상대적으로 작을 뿐, Overhead다.
- Kernel이 각 Thread를 개별적으로 관리한다.
  - Process 내 thread들이 병행 수행이 가능하다. (장점)
  - 이 때 하나의 Thread Block이어도 다른 Thread는 작업 가능하다.

### 3. Multi-Threading
![image](https://user-images.githubusercontent.com/69780812/131221135-faa13152-43a1-48b3-92d4-d34a8d4d0c8e.png)
- n개 User Thread - m개 Kernel Thread (n > m)
  - User는 원하는 수 만큼 Thread를 사용한다.
  - Kernel Thread는 자신에게 할당된 하나의 User Thread가 Block 되어도 다른 User Thread 수행이 가능하다. Kernel이 관리하고 있다.
  - 병렬 처리가 가능하다.
  - 효율적이고 유연하게 처리할 수 있다.

## Summary
- Thread는 자원을 공유해서 작업 효율을 높인다.
- Thread는 자신만의 제어 요소를 가진다. (CPU Core가 여러개이면 각 Thread가 Core를 나눠서 쓴다. **병렬처리**)



