# PCB와 Context Switching

## 프로세스
> 프로세스는 운영 체제(OS) 안에서 여러개가 존재하며 동작한다.

프로세스는 실행 중인 프로그램을 의미하며 또한 운영체제로부터 시스템 자원을 할당 받는 작업의 단위이다.

#### 프로세스 메모리 영역
- `Stack` : 매개변수, 지역변수 등 임시적인 자료
- `Heap` : 동적으로 할당되는 메모리
- `Data` : 전역 변수
- `Text` : Program 의 코드

.

## 프로세스 관리
> 구동중인 프로세스가 여러 개일 때, CPU 스케줄링을 통해 프로세스를 관리하는 것

CPU 들은 각 프로세스들에 대해서 구분할 수 있어야 관리가 가능하다.
따라서 각각 다른 프로세스들의 본연의 특징을 갖고 있는 **Process Metadata** 정보를 활용한다.

#### 프로세스 메타데이터

<img src="https://velog.velcdn.com/images%2Fhaero_kim%2Fpost%2F8985c46a-6544-4f0e-a625-1cc691623163%2Fimg%20(7).png" height="550px" width="450px"> <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMVv6X%2FbtrLy2kTWnl%2FSkYsB1Lqot2vIwKbz3K3KK%2Fimg.png" height="500px" width="550px">

- **`포인터`**
= 프로세스의 현재 위치를 저장하는 포인터 정보.

- **`프로세스 상태`**
= 프로세스의 각 상태 (생성(New), 준비(Ready), 실행(Running), 대기(Waiting), 종료(Terminated))를 저장.

- **`프로세스 번호`**
= 모든 프로세스에는 프로세스 식별자를 저장하는 프로세스 ID 또는 PID라는 고유 한 ID가 할당.

- **`프로그램 카운터`**
= 프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장.

- **`레지스터`**
= 누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보.

- **`메모리 제한`**
= 운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보를 포함. ex) 페이지 테이블, 세그먼트 테이블 등.

- **`열린 파일 목록`**
= 프로세스를 위해 열린 파일 목록을 포함.

.

## PCB(Process Control Block)
> 운영체제가 프로세스를 제어하기 위해 프로세스들의 메타데이터를 저장하는 곳

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5tmZc%2FbtqUnLvQf0W%2FPVZ1TLoN3mEWk5YkjLUd90%2Fimg.png" height="400px" width="450px"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0biNS%2FbtrLyYCTKIy%2F8sQYRMkPbGcVvegS84FnX0%2Fimg.png" height="400px" width="450px">

CPU 에서는 프로세스의 상태에 따라 프로세스 교체작업이 이루어지게 된다.

만약 어떤 프로세스로부터 인터럽트가 발생해서, 현재 프로세스가 잠시 대기 상태가 되고 인터럽트가 발생된 프로세스를 실행 상태로 교체할 때, 대기 중인 프로세스의 정보를 잃어버리게 되면 프로그램을 처음부터 다시 시작해야 한다.

따라서, 대기하다가 다시 실행할 때 대기 상태로 **바뀌기 직전의 실행 정보를 저장**해둔다면 다시 실행 상태로 돌아왔을 때 아무 일도 없었단 듯이 흘러갈 수 있을 것이다.

**이 동작을 위해 PCB 가 필요하다.**

 한 PCB 안에는 하나의 프로세스 정보가 담기게 되고 프로그램이 실행되어 메모리에 적재됐을 때 프로세스가 생겨나고, 프로세스 주소 공간에 `코드`, `데이터`, `스택`, `힙` 공간이 생성된다.
 이후 해당 프로세스의 메타데이터들이 PCB 에 저장된다.
 
*프로세스가 종료된다면 PCB 도 제거된다!*

.

## PCB 관리 방법

> Linked List 방식으로 관리한다.

PCB List Head에 PCB가 생성될 때마다 하나씩 데이터가 붙게 된다. 주소 값으로 연결이 이루어져있는 연결리스트이기 때문에 삽입과 삭제가 용이하다. 

즉, 프로세스가 생성되면 해당 PCB가 생성되고, 프로세스 완료 시 제거된다.  이렇게 수행중인 프로세스를 변경할 때 CPU의 레지스터 정보가 변경되는 것을 **Context Switching** 이라고 한다. 

.

## Context Switching
> 실행중이던 프로세스의 상태를 PCB 에 보관하고, 새로 들어오는 프로세스의 PCB 정보를 바탕으로 레지스터에 값을 적재하는 과정

<img src="https://afteracademy.com/images/what-is-context-switching-in-operating-system-context-switching-flow.png" height="650px" width="850px">

#### 왜 Context Switching이 필요할까?
컴퓨터가 매번 하나의 작업만 처리할 수 있다면? 해당 작업이 끝날때까지 다음 작업은 기다릴 수 밖에 없다. 또한 반응속도가 매우 느리고 사용하기 불편하다.


#### 작업이 동시에 작동하는 것처럼 하기 위해선?
Computer multitasking을 통해 빠른 반응속도로 응답할 수 있다.
빠른 속도로 작업을 바꿔가며 실행하기 때문에 사람의 눈으론 동시에 실행이 되는 것 처럼 보이게 되는 장점이 있다.
CPU가 Task를 바꿔 가며 실행하기 위해선 **Context Switching**이 필요하다.

#### 어떻게 Context Switching은 진행될까?
Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리되고 있다.

1. 현재 실행하고 있는 Task의 PCB 정보를 저장한다. (Process Stack, Ready Queue)
2. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행하는 것을 반복한다.

보통 인터럽트 발생, 혹은 현재 프로세스의 선점 허용 기간을 모두 소모한 상황, 입출력을 위해 대기하는 경우에 Context Switching이 발생하게 된다.

.

## Context Switching Overhead
> 오버헤드(overhead)는 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리 등을 말한다.

Context Switching을 하는 동안에는 CPU 가 아무것도 하지 못하게 된다. 따라서, 만일 쓰레드 및 프로세스의 개수가 엄청 많아져 Context Switching 이 빈번히 일어나게 된다면, 오버헤드가 잦아져 성능이 악화될 가능성도 있다. 이를 **Context Switching Overhead** 라고 한다.

.

*p.s*
*스케줄링은 다음에 알아보자.*
