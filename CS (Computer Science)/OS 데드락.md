# OS 데드락
### 1. Deadlock
* 데드락(Deadlock) : 일련의 프로세스들이 **서로가 가진 자원을 기다리며 block 되어 더 이상 진행될 수 없는** 상태
* 프로세스가 자원을 사용하는 절차
    1. **Request** : 자원을 요청하고, 다른 프로세스가 자원을 사용하고 있어서 받을 수 없다면 대기함
    2. **Allocate** : 자원을 받음
    3. **Use** : 프로세스가 받은 자원을 사용함
    4. **Release** : 프로세스가 자원을 놓아줌
* 데드락은 **모든 프로세스가 Request** 상태되어 있는 상황
### 2. Deadlock Characterization
* 데드락이 발생하기 위해선 아래 4가지 조건을 만족해야 함
1. **Mutual exclusion (상호 배제)**
    * 매 순간 하나의 프로세스만이 자원을 사용할 수 있음
2. **Hold and wait (보유 대기)**
    * 자원을 가진 프로세스가 다른 자원을 기다릴 때, 보유하고 있는 자원을 놓지 않고 계속 가지고 있음
3. **No preemption (비선점)**
    * 프로세스는 OS에 의해 강제로 자원을 빼앗기지 않음
4. **Circular wait (순환 대기)**
    * 자원을 기다리는 프로세스 간에 사이클이 형성되어야 함
    * ex) 프로세스 p0, p1, ... , pn이 있을 때 p0은 p1을 기다리고, p1은 p2를 기다리고, ..., pn은 p0를 기다린다.
    * 만약 자원 할당 그래프 (Resource-Allocation Graph)에 사이클이 없다면 데드락이 아니다.
    * 사이클이 존재한다면 데드락이 발생할 '수' 있다.
* 데드락 문제를 해결하기 위한 방법 중 대표적 방법 
  1. 미리 예방하는 방법(Prevention) 
  2. 데드락이 발생하지 않도록 피하는 방법(Avoidance) 
  3. 발생했을 때 처리하는 방법(Detection and Recovery) 
  4. 무시하는 방법(Ignorance)
### 3. Deadlock Prevention
### 4. Deadlock Avoidance
### 5. Deadlock Detection and Recovery
### 6. Deadlock Ignorance

