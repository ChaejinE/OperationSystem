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

> Register ?
  - Process 내부에 있는 **Memory**이며, 가장 빠른 메모리다.
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



