# Intro
- CPU, GPU, MainMemory(DRAM), LAN
- Operation System(OS)는 위 하드웨어들의 성능을 어떻게 효율적으로 사용할지를 결정해준다.
- 위 하드웨어들을 **Computer Resources** 라고하며 User들에게 **효과적으로 Service를 제공**하도하는 것이 Operation System이다.
- Processor : 일반적으로 CPU라고하며 Computing(연산)을 수행한다.
- Memory : 일반적으로 Data, Program 등을 Store(저장)한다.
- 주변장치 : Keyboard, Mouse, Monitor 등
# Computer Hard Ware
## 1. Processor
- 어떤 CODE에 대한 연산을 수행하며 모든 Computer 장치의 동작을 제어(Control)한다.

![image](https://user-images.githubusercontent.com/69780812/129758693-ee43ed1c-b2c9-4eab-8f2c-5c8e51430cf8.png)
- 보통 위와 같은 Flow로 동작한다.

```
Register ?
  - Processor 내부에 있는 **Memory**이며, 가장 빠른 메모리다.
  - 용도, 정보변경가능 여부, 저장 정보 종류에 따라 분류된다.
  - 정보변경가능 여부 (가시레지스터/비가시레지스터)
    - 가시 레지스터
      - 1. Data 레지스터 : 함수 연산에 필요한 데이터를 저장한다. (값, 문자)
      - 2. Address 레지스터 : 주소나 연산에 필요한 주소 일부를 저장한다.
    - 비가시 레지스터
      - 1. PC(Program Counter) : 다음에 실행할 명령어의 주소를 저장한다.
      - 2. IR(Instruction Register) : 현재 실행하는 명령어 주소를 보관한다.
      - 3. Accumulator : Data를 일시적으로 저장한다.
      - 뒤에서 더 설명 (간략하게 line by line으로 Write된 code를 해석하고, PC에 다음 실행할 코드에 대한 주소를 저장한다. 그 다음 IR에 전달)
```

***운영체제는*** 

***1. Processor에게 처리할 작업을 할당하고 관리한다.***

***2. Program이 Processor 사용할 시간을 관리한다.***

***ex) 복수의 Program간 CPU 사용 시간 조율***

## 2. Memory
- 데이터를 저장하는 공간이다.

![image](https://user-images.githubusercontent.com/69780812/129761549-7bd39792-fe41-48fb-8556-176879a213c0.png)
- Register -> Chache -> DRAM -> HDD
  - 왼쪽일 수록 속도가 빠르다.
  - 오른쪽일 수록 저장 공간이 많다.
  - **Processor는 DRAM(Main Memory)까지만 접근이 가능하다.**

```
Main Memory?
- DRAM, DDR4(최근 이걸 많이 사용한다고 한다.)
- Processor가 수행할 Program, Data를 저장한다.
- HDD같은 메모리 사용시 I/O Bottlenect 현상이 발생한다.
  - Main Memory가 Processor의 직접적인 접근을 하도록하여 이를 해결했다.
  - Main Memory는 저장 공간이 작지만 HDD보다는 훨씬 빨랐기 때문이다.
```
![image](https://user-images.githubusercontent.com/69780812/129762828-9df1cd5c-72c5-4a76-907d-7956c1a9bf85.png)

```
Cache ?
- CPU, Processor 내부에 있다.
- Main Memory보다 물리적으로 Core(CPU)에 더 가까이있다.
  - 데이터를 저장하고 읽는 속도가 더 빠르다.
  - Main Memory의 I/O Bottleneck 현상을 해소해줬다. (Main Memory도 데이터를 저장하고 읽는 속도를 높이는데 한계가 있었을 테니..)
  - 128KB 정도 되는 매우 작은 용량을 가지고 있다.
- 아니 이 따위 용량이 Bottleneck 현상을 해소는 할 수 있는 거야 ?
  - 기본적으로 Cache는 HW적으로 알아서 관리가 된다. Processor -> Query to Cache Data있어? -> 없으면 Main Memory로 Query해서 Cache로 가져온다., 있으면 Cache에서 처리 (아래 사진 참조)
- Cache Hit : 필요한 Data 블록이 Cache에 존재하는 경우
- Cache Miss : 필요한 Data 블록이 Cache에 없는 경우 -> 손해 보는 상황이다.
- 아니 Cache Hit 현상이 자주 일어나야 좋은거 아니야 ? 128KB로 얼마나 그런 현상을 바라는지.. 기도메타 아니야 ?
  - 이에 대한 근거는 Locality(지역성)이다.
  - 1. Spatial Locality : 참조한 주소와 인접한 주소를 참조할 가능성이 크다. MainMemory에서 Query후 Cache로 가져오는 Data는 block size 만큼 가져오므로 인접한 데이터도 가져오기 때문이다. (순차적 Program)
  - 2. Temperal Locality : 한번 참조한 주소는 다시 한번 참조할 가능성이 크다. (ex for문과 같은 순환문)
- Locality는 Cache Hit ratio(캐시 적중률)과 밀접한 연관이 있으며 알고리즘 성능 향상을 위한 중요한 요소 중 하나이다.
  - ex) 2차원 배열 연산 처리
  - Cache만 이해하더라도 Program 속도가 10~100배 향상될 수 있다.
```
![image](https://user-images.githubusercontent.com/69780812/129764544-6f86cce9-9775-46f7-99c4-0b848b5ddd95.png)

![image](https://user-images.githubusercontent.com/69780812/129764640-2c9c94c5-c525-4a46-8856-3fe9bafd8f1b.png)

![image](https://user-images.githubusercontent.com/69780812/129764721-cea69b77-e2d9-4850-bbaa-f4fb638199a4.png)

## 3. Auxilary Memory
- 마찬가지로 Program, Data를 저장한다.
- Processor가 직접 접근하는 공간이 아니다. 따라서 주변장치로 분류된다.
  - Main Memory에 Program 및 Data를 올려야된다.
- Main Memory가 8GB이고, Disk가 20GB 처럼 상대적으로 저장 곤간이 적은데, 어떻게 올린단 말인가 ?
  - Virtual Memory의 개념이 이에 대한 해답이 된다고 한다. (공부하면서 나중에 나온다.)

## 4. System Bus
- H/W들이 Data 및 Signal을 주고 받는 물리적 통로 (PATH)
- data bus : 배선 수가 **프로세서가 한번에 전송할 수 있는 비트 수를 결정**한다. (이를 word라고한다.)
- address bus : 배선 수가 **프로세스와 접속할 수 있는 CPU의 최대 용량을 결정**한다.
- control bus : 연산 장치의 연산 종류, CPU의 Read/Write 동작을 결정하는 신호들의 통로이다.

![image](https://user-images.githubusercontent.com/69780812/129765436-7b6d27ed-2466-40fe-822d-747c2162a812.png)

![image](https://user-images.githubusercontent.com/69780812/129765511-ad66a20c-dca0-4d9a-b64e-9e2d0ae8c8dd.png)
- 이 사진은 비가시 레지스터의 동작 Flow 이해에도 도움이 많이 된다.

## 5. 주변장치
- 입력 : 키보드, 마우스 등
- 출력 : 모니터 등
- 저장 : CD, USB 등

***OS는*** 

***1. 장치 드라이버를 통해서 이들을 관리한다.***

```
장치 드라이버 ?
- 주변 장치의 Vendor들이 제공하는 API이다.
```

***2. Interrupt 처리***

***3. File 및 Disk를 관리한다.***

***- File 생성 및 삭제***

***- Disk 공간 관리***


