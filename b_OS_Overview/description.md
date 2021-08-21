# OS 역할
## 1. User Inferface
1. CUI
2. GUI
3. EUCI : mp3화면 같이 특정 user에게 편의를 위해 제공되는 UI
## 2. Resource Management
- HW Resource : Processor, Memory, I/O devices ...
- SW Resource : file, application, meesage, signal ...
## 3. Process & Thread Management
- Process는 프로그램의 실행 주체다.
## 4. System Management
- 시스템 보호
# Multi-layer Architecure
- Process 들은 필요한 기능과 리소스를 요청하게된다.
- 이를 위해 Kernel에 직접 접근하는 것은 위험한 접근방법이 될 수 있다.
- System Call Interface를 통해 Kernel에 있는 유용한 기능들에 접근할 수 있다.
  - Kernel : System libraries가 모여있는 곳. 메모리에 상주하고 있다.

![image](https://user-images.githubusercontent.com/69780812/130318841-425b6c8c-de48-494d-8a2b-cca5d1621b6c.png)

![image](https://user-images.githubusercontent.com/69780812/130318870-b2eafe09-a79f-44c5-affe-f0569e44846f.png)

# OS 구분 방법
## 1. 동시 사용자 수
- Single-User System : Windows
- Multi-User System : Linux, Unix
  - **동시에** 여러 사용자들이 시스템을 사용한다.
  - 시스템 Resource에 대한 소유 권한 관리가 필요해진다.
  - 위의 특징 때문에 OS의 기능과 구조가 복잡하다.
## 2. 동시 실행 Process 수
- Single Tasking : MS-DOS
  - 한 번에 하나의 Process만 존재하는 경우
- Multi Tasking : Unix, Linux, Windows
  - **동시에** 여러 Task를 수행한다.
  - 위 특징 때문에 OS의 기능과 구조가 복잡하다.
## 3. 작업 수행 방식
![image](https://user-images.githubusercontent.com/69780812/130319163-6f04d27c-1cda-4c9a-9dda-167f4baf9c53.png)
- 최초에는 운영체제에 대한 개념이 존재하지 않았다. 모든 작업을 기계어로 직접 Program을 작성했었다.
- 줄서서 작업별 순차처리 -> 각 작업에 대한 준비시간 Issue
  - Program 언어가 다르면 Computer 다시 세팅해야되는 등..
- 사용자 요청 작업들을 모은다.

- ***1. Batch System (1950 ~ 60s)***

![image](https://user-images.githubusercontent.com/69780812/130319185-7d5a3fdc-179e-4f4a-a68f-7d9998a14f4c.png)
  - 사용자 요청 작업을 모은다.
  - 100개 모이면 한번에 처리하는 방식
  - **작업 준비 시간이 줄어드는 효과**가 있었다.
  - **System Oriented (시스템 지향적)**
    - **처리효율(Throuput)은 향상**
    - 같은 유형 작업이 모이길 기다려야해서 **생산성은 떨어졌고, 응답성(개인적)은 느렸다.**

- ***2. Timer Sharing Systemm***

![image](https://user-images.githubusercontent.com/69780812/130319200-91ff12c0-77e1-46b3-ac3e-5a12088b1fc9.png)
  - 여러 사용자가 자원을 동시에 사용하도록 했다.
  - 단지, 특정 시간동안 A 작업을 하다가 B작업으로 전환하여 특정 시간 반복
    - OS가 **File System & Virtual Memory를 관리하기 시작**한다.

![image](https://user-images.githubusercontent.com/69780812/130319247-43e37974-0d9e-4797-8c26-f98991e38100.png)
  - **User Oriented (사용자 지향적)**
    - 단말기를 사용해서 대화형으로 일을 처리했다.

![image](https://user-images.githubusercontent.com/69780812/130319353-32739b9e-9df2-4e8e-93d9-91636c07669b.png)
  - 응답시간이 단축되기는 했다. 5초 정도..
  - 배치처럼 특정 Task에 대한 Program 처리가 끝나고, 다른 Task로의 준비 시간이 없어져서 Process 가 쉴 시간이 줄어들었다. -> **생산성 향상**
  - 하지만, Process 간의 **통신 비용이 증가**했다.
    - 통신 개념이 중요해지면서 **보안 Issue**가 등장했다.
    - **동시 사용자 수 증가** -> **System 부하** -> 개인적으로 Task 처리가 **느려졌다.**

- ***3. Personal Comptuing***
  - 느려진 Computing .. 차라리 그냥 개인이 System 전체를 독점하자는 취지에서 나왔다.
  - CPU를 얼마나 사용하냐보다는 얼마나 편하게 쓰는지에 초점을 두고 발전했다.
    - 다양한 사용자 지원 기능을 지원하도록 했다. (ex) Windows 
  - 혼자쓰니까 **응답 시간은 빠르다** 하지만 **CPU성능은 상대적으로 낮아졌다.**

- ***4. Parallel Processing System***

![image](https://user-images.githubusercontent.com/69780812/130319747-41a003af-587c-447b-8898-2ce1c7cf755e.png)
  - CPU가 여러개이며, Memory등 Resource들을 공유하게 된다.
  - 자원을 동시에 공유해서 **Tightly-coupled System**이라고도 부른다.
  - **성능이 향상**되고, **신뢰성이 향상**되었다.
    - 왜 신뢰성이 향상 ? CPU중 하나가 죽어도 다른 CPU가 처리가 가능해졌다.
    - OS의 **프로세서간 관계 및 역할 관리**가 필요해졌다.

- ***5. Distributed Processing System***

![image](https://user-images.githubusercontent.com/69780812/130319762-6c529461-88ef-4678-a932-7e2cee6ea2cc.png)
  - CPU를 개인 COM에 다 넣기엔 한계가 있더라.. 100개를 넣을 수는 없으니..
  - Network 기반으로 여러 컴퓨터를 구축한 System이다.
  - Node(개인)들이 각자 OS를 따로 갖고있다.
    - 각 구성요소들의 **독립성 유지**하면서 **공동 작업**가능
  - 분산 운영체제를 통해 하나의 Program, 자원처럼 사용이 가능해질 수 있다.
    - (ex) Client - Server, Super Computer
  - 자원 공유를 통해 **성능 향상**, 독립성 유지를 통해 시스템의 **신뢰성이 향상**되었다.

- ***6. Real-Time System***
  - 작업처리에 제한시간(dead line)을 갖는 System이다.
  - **제한 시간 내 Service를 제공하는게 자원 활용보다 중요하다.**
  - Hard Real Time Task : 시간 제약을 지키지 못하는 경우 System에 치명적
    - (ex) 발전소 제어, 무기 제어
  - Soft Real Time Task
    - (ex) 동영상 재생 -> 1초에 30Frame만드는 system -> 통신 불안정 -> 영상 매끄럽지 못함

    - ![image](https://user-images.githubusercontent.com/69780812/130319933-fefeb652-d348-4856-994b-aa292a138be7.png)
      - 제한 시간 내 처리하지 못해도 재앙적이지는 않은 경우다.
  - Non Real Tme Task : 그 외

# OS 구조
![image](https://user-images.githubusercontent.com/69780812/130319966-4716992c-0088-4246-b33f-e95525cb2c1b.png)
- 1. Kernel
  - OS의 핵심이다.
  - 가장 빈번하게 사용되는 기능들을 모아놨다.
    - System Management (Process, Memory 등 Management)
    - 따라서, **항상 Memory에 상주**해있다.
- 2. Utility
  - 비상주 Program이다.
    - 필요할때만 상주하게된다.
  - Computer log or 작업 관리자
  - UI등 서비스 프로그램

## 1. Single Architecture (단일 구조)
![image](https://user-images.githubusercontent.com/69780812/130320102-ecdec94a-9702-4700-be05-644bba2715d5.png)
- 장점
  - 1. 커널 내 모듈간 **직접 통신** -> **빠르고, 효율적 자원관리**
- 단점
  - 1. 커널 거대화 -> **오류 & 버그** -> **유지 보수가 어려워진다.**
  - 2. 동일 메모리에 모든 기능 -> 악성 코드 등으로 인해 **한 모듈의 문제가 전체 System에 영향 끼칠 가능성 높다.**

## 2. Layer Architecture (계층 구조)
![image](https://user-images.githubusercontent.com/69780812/130320120-138e4ee7-b598-4bb5-be14-ce1d508a75af.png)
- 모듈화를 통해 기능을 분리하여 기능별로 기능을 구현했다.
  - **설계, 구현이 단순화** 되었다.
  - 하지만, 원하는 기능 호출을 위해서는 여러 계층을 거쳐야 하므로 **성능은 다소 저하**되었다.

## 3. Micro Kernel
![image](https://user-images.githubusercontent.com/69780812/130320149-5973fbd2-e9e0-44b6-a0c1-7f2f54b0cffc.png)
- Kernel 크기를 최소화했다.
  - 진짜 필수 기능만 남기고 나머지는 User 영역에서 수행하도록 한 구조

# OS 기능
## 1. Process 관리
- ***Process***
  - **Kernel에 등록된 실행단위** 이다.
  - 실행 중인 프로그램이다.
  - User의 요청에 대한 처리 또는 기능을 수행하는 **수행의 주체**다.
- OS는 Process를 생성/삭제 하며 상태를 관리한다.
- OS는 Process 실행을 위한 자원을 할당한다.
- OS는 Process들 간의 교착상태를 해결한다.
- OS는 Process Control Block을 통해 Process 정보를 관리한다.

## 2. Processor 관리
- Process에 대해 스케쥴링한다.
  - System 내 **Process 처리 순서를 결정**한다.
- Processor는 한번에 하나의 Process만 사용할 수 있다. 그래서 OS는 Processor 할당을 관리 한다.

## 3. Memory 관리
- Main Memory(DRAM)
- Process에 대한 **Memory를 할당하고, 회수**한다.
- 여유 공간을 관맇나다.
- 각 프로세스의 할당 메모리 영역에대한 접근을 보호한다.

## 4. File 관리
- 대표적인 SW Resource로 **논리적 Data 저장단위**이다.

## 5. I/O 관리
![image](https://user-images.githubusercontent.com/69780812/130320273-f8184eed-a8f8-4291-a7af-a2bae399d728.png)
- 입출력은 반드시 OS를 거치게 된다.

