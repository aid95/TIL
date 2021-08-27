# 운영체제가 뭐길래?

## 운영체제가 뭐길래?

운영체제란, **컴퓨터 하드웨어를 관리하는 소프트웨어**이다. 그렇다면 컴퓨터란 어떤 기준으로 나뉠까? 계산기도 컴퓨터? 스마트폰은 컴퓨터? 이 물음에서 **정보를 다루는 기계**와 같은 기준을 들 수 있다.

그렇다면 정보란 무엇인가? 정보는 불확실한 정보를 측정하고 수치로 표현한 것이다. 컴퓨터에서 이런 정보의 최소 단위는 _**bit**_이다. 이런 정보의 처리는 0과 1의 상태 변환을 통하고 이런 상태 변환을 부울 대수\(NOT, AND, OR, etc...\)을 통해 가능하다.

* 덧셈 : 반가산기, 전가산기
* 뺄셈 : 2의 보수 표현법
* 곱셈/나눗셈 : 덧셈과 뺄셈의 반복
* 실수 연산 : 부동 소수점 표현법
* 함수 : GOTO

이런 정보 처리를 통해 모든 정보 처리를 할 수 있게 된다.

### 컴퓨터

* 범용성
  * NOT, AND, OR 게이트만으로 모든 계산을 할 수 있다.
  * NAND 게이트만으로 모든 게산을 할 수 있다.
* 계산가능성
  * Turing-computable : 튜링 머신으로 계산 가능한 것
  * 정지 문제 : Halting Problem, 튜링 머신으로 풀 수 없는 문제

컴퓨터의 할아버지 Alan Turing의 Turing Machine의 원형이 컴퓨터이며, John von Neumann의 ISA\(Instruction Set Architecture\)을 통해 실제 컴퓨터가 탄생하게 되었다.

#### Turing Machine

앨런 튜링이 제안한 튜링 머신은 현대 컴퓨터의 CPU, 메모리, 운영체제, 응용 프로그램 형태의 모습과 매우 유사하다. 이런 튜링 머신을 폰 노이만의 메모리에 _명령어들의 집합_인 프로그램을 저장하고 불러와 실행하는 Stored-program 방식의 _**폰 노이만 아키텍쳐**_ 도입하여 현대의 컴퓨터가 실제로 탄생하게 된다.

#### 운영체제도 프로그램?

운영체제도 프로그램이다. 하지만 다른 애플리케이션 프로그램과 몇가지 차이점이 있다.

* 항상 실행중인 프로그램
* 애플리케이션 프로그램에 시스템 서비스 제공
* _**프로세스**_, 리소스, 사용자 인터페이스\(마우스, 모니터, 키보드 등등\)를 관리

## 운영체제의 구조와 개념

전통적인 컴퓨터 시스템은 다음과 같은 특성을 가진다.

* 하나 또는 그 이상의 CPU
* 여러개의 디바이스 컨트롤러와 버스를 통한 연결

### 컴퓨터의 부팅

이런 운영체제은 소프트웨어이며 폰 노이만 구조에선 메모리에 프로그램이 올라와야 CPU에 의해 명령어가 수행될 수 있다. 하지만 메모리는 휘발성이며 컴퓨터의 전원이 꺼지면 사라지는데 이런 _**커널**_을 메모리에 올라오기 위해선 부팅시 가장 먼저 실행되는 Bootstrap 프로그램에 의해 수행된다.

### Interrupts

CPU를 충분히 사용하며 성능 저하가 없이 불규칙적인 이벤트를 처리하기 위하여 이벤트가 발생하면 CPU에게 시스템 버스와 같은 통로를 통해 신호를 전송하여 작업을 처리하는 개념

### 폰 노이만 아키텍쳐

명령어 실행 사이클이 있으며 메모리로 부터 하나의 명령어를 가져오는 것을 **fetch**라고 한다. fetch는 _**instruction register**_가 가리키는 명령어를 가져오며 이를 디코드하여 **실행**한다.

### Symmetric multiprocessing \(SMP\)

현재 대부분의 컴퓨터는 하나의 CPU, 하나의 램으로 구성된 컴퓨터는 특수한 상황이 아니라면 사용되지 않는다. 따라 CPU processor들은 모든 작업에 대해 동시에 처리할 수 있다.

#### Multi-core design

이처럼 프로세스 자체를 여러개 사용할 수 있지만, 하나의 프로세스 내부에 여러개의 코어를 구성하는 멀티 코어 디자인도 존재한다.

### 멀티 프로그래밍

앞서 프로세스, 코어가 하나 이상이 된 현대에는 여러개의 프로세스를 동시에 수행할 수 있는 멀티 프로그래밍을 가능하다. 따라서 메모리에 여러개의 프로세스가 올라가 있어 동시에 수행될 수 있다. CPU의 입장에서는 하나의 작업을 처리하기엔 너무 많은 시간을 사용하지 못하기 때문에 여러개의 작업을 시분할하여 마치 모든 프로그램이 동시에 실행되고 있는 것 처럼 하여 CPU의 사용 효율을 높일 수 있다.

**CPU scheduling**

하지만 여러개의 작업이 동시에 실행하게 되면 다음 작업을 선정하는데 문제가 있다. 이 내용은 이후에 다루게 된다.

### 운영체제 모드

운영체제에서는 하드웨어 제어와 리소스 관리와 같은 크리티컬한 작업을 수행하기 때문에 이런 작업이 사용자에게 노출되어 직접 수행되는 것을 막을 필요성이 있다. 따라서 _user mode_와 _kernel mode_로 분리하여 접근 범위를 제한하여 시스템의 안정성을 만족시키고 있다.

### 가상화

가상화는 다음과 같은 기술을 가능케한다.

* 단일 컴퓨터에서 하드웨어의 추상화
* 서로 다른 컴퓨터 환경을 구축

가상화 매니저는 서로 다른 운영체제 환경을 하나의 컴퓨터에서 동시에 구성하고 실행할 수 있도록 중간에서 호스트의 언어로 처리할 수 있도록 하지만 많은 리소스와 연산 비용을 가지고 있기 때문에 성능을 높이기 위해선 호스트의 높은 하드웨어 스펙을 요구하고 제한이 있다.

### 운영체제가 제공하는 서비스

* 사용자 인터페이스
* 프로그램 실행
* I/O
* 파일 시스템
* 커뮤니케이션
* 오류 검출
* 자원 할당
* 보안
