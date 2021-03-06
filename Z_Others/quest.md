## Contents
* [이산수학](#이산수학)
* [컴퓨터 구조](#컴퓨터구조)
* [네트워크](#네트워크)
* [운영체제](#운영체제)

<br/><br/><br/>


## 이산수학
### 1001 1010의 1의 보수?
* 정답: 0110 0101
### N의 보수 / 1의 보수 / 2의 보수 (N's complement)
* 1의 보수를 사용하다 보면 문제가 있다. 👉 0이 2개가 생긴다. 
  * +0: 0000 0000
  * -0: 1111 1111
  * 👉 2의 보수의 탄생 : 2의 보수 = 1의 보수 + 1
  * 👉 한 바이트의 범위는 -128~127
* ex) '1'의 2의 보수
  ```
  0000 0001 → 1111 1110 → 1111 1111
     <1>      <1의 보수>  <2의 보수>
  ```
### N의 보수
* 모든 진법에는 보수가 있다.
> `R진수`에 대해 `R의 보수`와 `R-1의 보수`가 존재한다.  

ex) 10진수 424
* 10의 보수: 10^3-424 = 576
* 9의 보수: (10^3-1)-424 = 575

### 비트연산자
#### - 특정 비트 켜기: 플래그 |= 마스크
* OR 연산 | 
  * 0 | 1 = 1
  * 1 | 1 = 1

``` c
int main() {
  unsigned char flag = 0;
  
  flag |= 1; // 0000 0001
  flag |= 2; // 0000 0010
  flag |= 3; // 0000 0100
}
```
#### - 특정 비트 끄기: 플래그 &= ~마스크
* AND 연산 &
  * 0 & 1 = 0
  * 1 & 1 = 1
``` c
int main() {
  unsigned char flag = 7; // 0000 0111
  
  flag &= ~2; // 1111 1101
}
```
#### - 특정 비트 토글: 플래그 ^= 마스크
* XOR 연산 ^
  * 0 ^ 1 = 1
  * 1 ^ 1 = 0
  * 0 ^ 0 = 0
``` c
int main() {
  unsigned char flag = 7; // 0000 0111
  
  flag ^= 2; // 0000 0010
}
```

<br/>
<br/>

## 컴퓨터구조
### 컴퓨터 하드웨어 기본 구성요소 3가지
* 기억장치 (RAM, HDD)
* 중앙처리장치 (CPU)
* 입출력장치 (마우스, 모니터, 프린터)

#### CPU를 도와 스크린 화면의 화상을 제어하는 그래픽 전용 프로세서
* GPU

#### 일반적인 운영체제에서 가상메모리를 사용하기 위해 메인메모리와 하드디스크 사이에 한 번에 이동하는 단위
* 페이지(Page)

<br/>
<br/>

## 네트워크
#### OSI 모델에서 다른 네트워크와 통신하기 위한 경로 설정을 위해 라우팅하며 패킷 전송 담당하는 계층은?
* 네트워크 계층

#### MAC 주소는 48비트

#### 네트워크 계층 주소와 데이터 링크 계층 주소 사이의 변환을 담당하는 프로토콜로 IP주소를 물리 주소인 MAC 주소로 변환하는데 사용하는 프로토콜은?
* 주소 결정 프로토콜(Address Resolution Protocol, ARP)


<br/>
<br/>

## 운영체제
#### 선점형 스케줄링
* 어떤 프로세스가 CPU를 할당받아 실행 중이더라도 운영체제가 CPU를 강제로 빼앗을 수 있는 스케줄링
1. 라운드 로빈 (Round Robin)
   * 프로세스는 같은 크기의 CPU 시간을 할당
   * 할당된 시간 내 처리 못하면 준비 큐 리스트의 가장 뒤로 보내지고 CPU는 대기중 인 다음 프로세스로 넘어간다.
   * 균등한 CPU 점유시간, 시분할 시스템 사용

  
2. SRT (Shortest Remaining Time First)
   * 가장 짧은 시간이 소요되는 프로세스 먼저 수행
   * 준비 큐에 남은 처리시간이 더 짧다고 판단되는 프로세스가 생기면 그 프로세스가 선점한다.


3. 다단계 큐 (Multi Level Queue)
   * 작업들을 여러 종류 그룹으로 분할
   * 여러 큐 이용하여 상위단계 작업에 의한 하위단계 작업이 선점 당한다.
   * 각 큐마다 자신만의 독자적인 스케줄링
   * 독립된 스케줄링 큐


4. 다단계 피드백 큐 (Multi Level Feedback Queue)
   * 입출력위주와 CPU위주인 프로세스의 특성에 따라 큐마다 서로 다른 CPU 시간 할당량 부여
   * FCFS (FIFO)와 라운드로빈 혼합
   * 새로운 프로세스일 수록 높은 우선순위, 실행기간이 길어질수록 점점 낮은 우선순위 큐로 이동
   * 마지막 단계는 라운드 로빈 방식 적용

#### 비선점형 스케줄링
1. 우선순위 (Priority)
   * 프로세스 별 우선순위가 주어진다.
   * 우선순위에 따라 CPU할당
   * 동일 순위는 FCFS
   * 주요/긴급 프로세스는 우선 처리
   * 설정/자원상황에 따라 우선순위 설정
2. 기한부 (Deadline)
   * 작업들이 명시된 기간 또는 기한 내 완료되도록 계획
   * 명시된 시간 내 처리 보장
3. FCFS (First Come First Service)
   * 대기 큐 도착 순서에 따라 CPU 할당
   * FIFO 알고리즘
4. SJF (Shortest Job First)
   * 프로세스가 도착하는 시점에 따라 그 당시 가장 작은 서비스 시간을 갖는 프로세스가 종료할 때 까지 자원 점유
   * 준비 큐 작업 중 가장 짧은 작업부터 수행
   * 평균 대기시간 최소
   * CPU 요구시간이 긴 프로세스는 기아현상 발생 (Starvation) 
5. HRN (Highest Response Ratio Next)
   * 대기중인 프로세스 중 현재 응답률(response ratio) 제일 높은 것 선택
   * SJF의 약점인 기아 현상 보완한 기법
   * HRN의 우선 순위 = (대기시간+서비스시간)/서비스시간



### 상호배제 (Mutual Exclusion)
* 2개 이상의 프로세스가 동시에 공유자원을 엑세스하는 코드 영역에 진입하지 못하도록 함
* 여러 프로세스가 동시에 공유자원을 사용할 때 각 프로세스가 번갈아가며 공유자원을 사용하도록 임계구역을 유지하는 기법
* 대표적인 방법
  * 데커 알고리즘 (두 개의 프로세서의 상호배제문제를 하드웨어의 도움없이 소프트웨어로 해결한 방법)

### 임계구역 (Critical Section)
* 다중 프로그래밍 운영체제에서 여러 개의 프로세스가 공유하는 데이터 및 자원에 대하여 **어느 한 시점에서는 하나의 프로세스만
  자원 또는 데이터를 사용하도록 지정**되는 시점입니다.
* 임계구역 에서는 하나의 프로세스만 접근할 수 있으며 해당 프로세스가 자원을 반납한 후에만 다른 프로세스가 자원이나 데이터를 사용할 수 있다.

### 교착상태 (Deadlock)
* 상호배제 (Mutual Exclusion)에 의해 나타나는 문제점
* 둘 이상의 프로세스들이 자원을 점유한 상태에서 서로 다른 프로세스가 점유하고 있는 자원을 요구하며 무한정 기다리는 현상

<br/>
<br/>




