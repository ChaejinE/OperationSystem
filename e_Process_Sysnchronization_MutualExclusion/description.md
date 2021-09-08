# Process Synchroninzation
- Multi-Programming System : Process가 많다.
- Process들은 독립적으로 동작하기 때문에, 공유 자원을 다룰 때 문제가 발생할 수 있다.
- **Synchronization** : Process들이 **서로 동작을 맞추거나** **정보를 공유**하는 것
  - Process간 대화를 통해 진행한다.
# Asynchronization and Concurrent Process
- **Asynchronization** : Process들이 서로에 대해 모르며 각각 행동하는 것을 의미한다.
- **Concurrent** : 여러 Process들이 동시에 System에 존재하는 것을 의미한다.
- Concurrent 작업을 하는 Process들이 Asynchronization Process들이라면, 정보 및 자원 공유 시 동시에 접근하는 문제가 발생한다.
  - 동기화의 필요성
# Terminologies
## 1. Shared data
- Process 들이 공유하는 data
- Critical data라고도 한다.
## 2. Critical section
- Shared data에 접근하는 Code Segment(영역)
## 3. Mutual Exclusion
- 둘 이상의 Process가 동시에 Critical section에 진입을 막는 것
## 4. Race Condition
- 명령 수행 순서에 따라 결과가 달라지는 것
## 5. Primitive
- 어떤 목적을 위한 **기본연산** 이라고 생각하면된다.

# Machine Instruction의 특성
- Automicity, Indivisible
  - 기계 명령 진행 중 방해받지 않으며 멈추지 않고 실행된다는 것을 의미한다.

![Screenshot from 2021-09-08 10-01-20](https://user-images.githubusercontent.com/69780812/132428964-ddf01d02-eea6-48d3-aa9a-2bfd9aa5bef0.png)
- 하나의 명령 중에는 방해 받지 않지만, 누가 먼저 데이터에 접근해서 연산을 처리하냐에 따라 기대하는 값이 다를 수 있다.
![Screenshot from 2021-09-08 10-08-03](https://user-images.githubusercontent.com/69780812/132429454-3a72f6fa-1bc8-4525-b5de-15003d2c5c9e.png)
- 기계명령으로 바뀌었을 때 Critical Section 상호배제 연습

# Primitives
- enterCS(Critical Section) Primitive
  - Critical Section에 진입 전, 다른 Process가 있는지 검사
- exitCS Primitive
  - Critical Section에서 벗어 날 때 후처리 과정
  - 벗어남을 System에 알린다.
## Requirements
1. Mutual Exclusion
- CS에 Process 있으면 다른 Process들은 진입을 금지한다.
2. Progress
- CS안에 있는 Process 외에 다른 Process가 진입하는 것을 방해하면 안된다.
3. Bounded Waiting
- CS진입은 유한 시간 내 허용해야한다.

