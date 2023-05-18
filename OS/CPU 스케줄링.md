# 스케줄링
## 개념
`프로세스들의 실행 순서를 정해주는 것`이다. **준비 상태**에 있는 프로세스 중에서 어느 프로세스에게 CPU를 할당할 것인지 결정한다.

## 필요성
계산을 할 때는 CPU가 필요하지만, I/O를 할 때는 필요 없다. 이때, CPU를 다른 프로세스가 이용하면 전체적인 시스템의 효율은 올라간다. 즉, `CPU를 여러 프로세스가 번갈아가면서 사용하면 효율적일 수 있다`.

## 스케쥴러의 종류
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/f6fca19b-265b-4318-b706-a0e3bdef67be)


### 단기 스케줄러
- 메모리 내부에 있는, **실행 준비(Ready) 상태 프로세스 중에서 한 프로세스를 선택하여 CPU에 할당**하는 것을 의미한다.
- 일반적인 스케줄러라 함은 단기 스케줄러를 의미한다.

### 중기 스케줄러
- 메모리에 프로세스가 너무 많으면, 메모리에 있는 프로세스를 선정하여 디스크로 보낸다.
- 시간이 흘러 메모리에 여유가 생기면 메모리에 다시 적재한다. (Swap-in)

### 장기 스케줄러 (작업 스케줄러)
- 스케줄러 알고리즘에 따라 `어떤 프로그램을 하드디스크로부터 메모리로 적재`할지 결정하는 것을 의미한다.
- 생성(상태)에 있는 프로그램들 중에서 필요한 프로세스를 생성하여 Ready-Queue에 넣는다.
- 디스크와 같은 저장장치에 작업들을 저장해놓고 필요할때마다 실행할 작업을 레디 큐에서 꺼내 메모리에 적재한다.

**요약**
- **단기 스케줄러**는 CPU와 메모리 사이의 스케줄링을 담당하고, **장기 스케줄러**는 메모리와 디스크 사이의 스케줄링을 담당한다.
- **중기 스케줄러**는 스와핑(Swapping)을 통해 프로세스들이 CPU 경쟁이 심해지는 것을 방지하는 역할을 한다.

### 용어 정리
* **Throughput:** 단위 시간당 완료되는 작업 수
* **Turnaround Time**: `Completion Time - Arrival Time`
    * 프로세스가 완료된 시간에서 프로세스가 도착한 시간을 뺀 시간, 어떤 프로세스가 완료될 때까지 걸린 시간
* **Response Time(응답 시간)**: FirstRun Time - Arrival Time
    * 프로세스가 처음 실행되는 시간에서 프로세스가 도착한 시간을 뺸 시간
    * 예를 들어, P1 프로세스가 0초에 도착하여 5초에 처음 실행되었다면 응답 시간은 5초가된다.
* **Waiting time (대기시간)**: 준비 큐에서 기다린 시간

## 스케줄링 알고리즘
### 비선점형 스케쥴링
### 1. FIFO (First In, First Out) : non-preemptive 스케줄러
처음 들어온 프로세스가 처음으로 실행된다. 들어온 작업이 무엇이든 먼저 들어온 작업이 있다면, 그 작업이 수행된 뒤에 새로운 작업이 수행된다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/6aed763a-8a8f-49ea-8075-1091ab388cbb)

위의 예는 A, B, C가 모두 0초에 도달했다고 하면
A는 Turnaround Time이 100초, Response Time은 0초이다.

B는 Turnaround Time은 110초, Response Time은 100초이다.

C는 Turnaround Time은 120초, Response Time은 110초이다.

평균으로, **Turnaround Time의 평균은 110초, Response Time의 평균은 70초**이다.

**예시**

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/16adfb10-06ab-40aa-8154-085743cd471c)

위의 그림에서 철수는 껌만 계산할려고 하는데, 먼저 계산할려고 해도 FCFS 방법은 먼저 온 요청만 처리하기 때문에 먼저 줄 서 사람부터만 계산을 할 수 있다.

FCFS 방법은 먼저 온 요청을 처리하기 때문에, 대단히 합리적인 스케줄링 방식인 것 같지만 **경우에 따라 비효율적인 결과를 초래**하기도 한다. *(이러한 문제점을 Convoy effect 발생 문제라 한다.)*

### 2. SJF (Shortest Job First) : non-preemptive 스케줄러

수행시간이 짧게 걸리는 프로세스부터 순서대로 실행하는 스케줄러이다.
FIFO에서 발생한 convoy effect를 해결하는 방법으로 수행시간이 짧은 애들부터 수행하려고 한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/2c36518e-c087-48d1-900a-5067b75e8907)

A,B,C는 모두 0초에 도착했고, 아까와 다른점은 동시에 도착했을 때 알파벳 순으로 수행하는 것이 아닌 수행 시간이 짧은 애들을 먼저 실행한다.

A는 Turnaround Time은 120초, Response Time은 20초이다.

B는 Turnaround Time은 10초, Response Time은 0초이다.

C는 Turnaround Time은 20초, Response Time은 10초이다.

평균을 구해보면 **Turnaround Time의 평균은 50초, Response Time의 평균은 10초**이다.
> Turnaround Time의 평균이 줄어들게 된 것을 볼 수 있다. SJF는 항상 가장 짧은 프로세스를 먼저 실행하기 때문에 항상 최적의 Turnaround Time을 가져다 줄 수 있다.

**하지만 단점은 이를 없애버릴 정도로 크다.**

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/93cdb852-4a4f-4657-9f5b-346c3ccb9150)

아까와 달라진 점은 B,C의 도착 시간이 0초가 아니고 `10초`라는 점이다 .

A는 0초에 도착한 100초짜리 작업, B,C는 10초에 도착한 10초짜리 작업이다. 
1. 0초에서 SJF로 프로세스를 스케줄링 하려고 봤더니, A 프로세스만 존재해 A를 수행한다.
2. 10초에 B, C가 도착하지만 작업은 한 번 실행되면 멈출 수 없다. 그래서 결국 B,C는 겨우 10초 늦은 이유로 A가 모두 수행되기를 기다려야 한다.

A의 Turnaround Time은 100초, Response Time은 0초이다.

B의 Turnaround Time은 100초, Response Time은 90초이다.

C의 Turnaround Time은 110초, Response Time은 100초이다.

평균을 구해보면 `Turnaround Time의 평균은 약 103.3초, Response Time의 평균은 60.3초`이다. 

즉, 어떤 작업이 현재 수행되고 있는 작업보다 짧아도 늦게 도착하면 수행되기 위해 기다려야한다. 이를 어떻게 해결할 수 있을까?

**예시**

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/45a303c8-e1a8-49a3-b247-b018c57f357b)

철수는 A 아주머니가 계산하는 도중에 껌을 계산하기 위해 A 아주머니 바로 뒤에 설 수 있다. 왜냐면 껌 하나만 들고 있으니까 CPU burst time이 짧기 때문이다.

만약에 A 아주머니가 물건을 1000개 들고있는데 아직 2개까지 계산을 안하면 계속 기다려야한다. 그래서 먼저 계산하고 싶다고 말하고 싶지만, SJF 방식은 비선점형 방식이기 때문에 계산하고 있다면 중간에 교체가 안된다.

즉, B 아주머니와 여러분의 위치만 순서를 정할 수 있는거지, A 아주머니가 계산대를 이용하고 있으면 끝날때까지 기다려야한다.

### 선점형 스케줄러
> 선점 스케줄러는 어떤 작업을 수행하는 도중에 다른 작업을 수행할 수 있도록 스케줄링할 수 있는 스케줄러를 말한다.
### 1. STCF (Shortest Time-to-Completion First) : preemtive 스케줄러
`특정 시점에서 가장 빨리 수행될 수 있는 프로세스를 먼저 실행`하는 스케줄러

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/cb7c0f2c-c549-4381-902f-7b2dc5b9f24f)

STCF는 10초에서 A를 계속 수행하는 것이 아닌, 그 시점에서 가장 빨리 수행될 수 있는 B,C가 먼저 수행되는 것을 볼 수 있다.

A의 Turnaround Time의 평균은 50초, Response Time은 0초이다.

B의 Turnaround Time은 10초, Response Time은 10초이다.

C의 Turnaround Time은 20초, Response Time은 10초이다.

**평균을 구해보면 Turnaround Time은 20초, Response Time은 10초**이다.
아까 SJF를 사용할 때보다 더 효율적으로 동작하는 것을 볼 수 있다.

**예시**

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/7697be3b-8a5e-44a6-80d2-cdb0d41a7d98)

이제 철수는 A 아주머니한테 양해를 구해 껌을 바로 계산할 수 있다. 그리고 B 아주머니의 사과가 더 작기 때문에 A 아주머니한테 양해를 구해 먼저 게산할 수 있다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/2c0a5c2e-8421-408b-aa67-fc9961ac22bc)

그런데 만약 껌만 사려고 하는 사람이 계속해서 줄을 서면, A 아주머니는 **영원히 계산 못하는 상황이 발생**할 수 있다.

이러한 상황을 `기아현상`이라고 한다.

### 2. SJF에서의 Response Time 구하기
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/578dc777-f14e-4684-bc7d-244ac325b5e5)

위의 예는 A, B, C가 동시에 도착했고, 모두 5초의 실행시간을 가진 작업이다. 

Response Time으로 A는 0초, B는 5초, C는 10초 걸렸다. 평균 Response Time은 5초이다.

> 만약에 B, C 작업이 마우스, 키보드 동작이였다면 어떤 현상이 벌어질까? 또, 어떻게해야 이 문제를 해결할 수 있을까?

### 3. Round Robin : preemptive 스케줄러
RR은 어떤 작업이 수행될 때 끝날 때까지 수행하는 것이 아닌, `정해진 시간 만큼만 수행하는 스케줄러`이다.
**정해진 시간을 수행한 뒤에는 다른 작업을 수행**하게 된다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/6d7c8ef4-213e-4da0-92b8-860c7156f163)

A, B, C가 0초에 동시에 도착했으며 모두 5초의 실행시간을 가진 작업이다.
이때 time slice가 1초로 설정된 RR로 처리하면 아래와 같이 결과가 나타나게 된다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/09312385-071e-4603-9bfd-ede5b7c14b87)

A의 Turnaround Time은 13초이다.
B의 Turnaround Time은 14초이다.
C의 Turnaround Time은 15초이다.

평균 Turnaround Time은 14초가 된다.

> 아까 SJF를 수행할 때 평균 Turnaround Time은 10초였는데, RR을 사용하니 Turnaround Time이 더 나빠진 것을 볼 수 있다.
> 즉, RR이 모든 기준에서 가장 좋은 방법이 아니라는 것을 알 수 있다.

반면에, Response Time은 개선이 되었다. 
A는 0초, B는 1초, C는 2초로 평균 RT는 1초이다.

**만약에 Response Time을 더 줄이고 싶다면, time slice를 더 줄이면 된다.**
> 하지만, Time slice를 최대한으로 줄이는게 마냥 좋은 것은 아니다.
> **Time slice가 너무 작게되면, context switch가 너무 자주 발생**하게 되고, context switch를 하는데 필요한 비용이 전체 성능에 큰 영향을 미칠 수도 있다. 

요악하자면 `SJF`, `STCF`는 **Turnaround Time을 최적화하지만 Response Time은 좋지 않다.**
두번째 유형인 `RR`은 **Response Time은 좋지만, Turnaround Time이 좋지 않다.**

### 4. Multi-Level Feedback Queue : Preemptive 스케줄러

* `SJF`, `STCF`는 Turnaround Time이 좋지만, 이를 사용하기 위해서는 프로세스의 수행 시간을 알고 있어야 했지만 알 수 없었다.
* RR과 같은 경우에는 Response Time은 좋지만 Turnaround Time은 아주 나쁜 성능을 보였다.

이런 문제들을 해결하기 위해, `MLFQ`가 나왔다.
> 실행할 프로세스에 대해 아무 정보가 없어도 스케줄링을 효율적으로 할 수 있도록 하고, 프로세스가 과거에 수행된 적이 있다면 그러한 정보를 토대로 더 나은 스케줄링을 할 수 있는 방법

### 적용되는 규칙
1. **우선순위가 높은** 프로세스들을 먼저 수행한다.
2. 작업들이 같은 우선순위를 갖는다면, `RR`로 수행한다.
3. **새로운 프로세스**가 시스템에 들어가면 가장 높은 우선순위를 부여한다.
4. 작업은 모든 우선순위에서 주어진 **time slice를 모두 사용하면 우선순위가 감소**한다.
5. 일정 시간 후 **시스템의 모든 작업을 우선순위가 가장 높은 큐로 이동**한다. (Priority Boost)

### 규칙 1, 2
우선순위를 변경해주지 않으면 발생하는 문제

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/7e5ead5f-90f0-4675-9703-7c4abb0a2579)

MLFQ는 우선순위에 따라 스케줄링 된다고 했으니, A, B가 가장 먼저 RR 방식으로 수행될 것이다.
그러면 **A, B가 완료될 때 까지는 C, D는 스케줄링 되지 않는다**는 말이다.
만약 A, B가 아주 오래걸리는 작업이면 C, D의 Response Time은 급격히 나빠지는데 이러한 현상을 어떻게 방지할 수 있을까?

### 규칙 3, 4의 등장
> 우선 순위를 어떻게 바꾸는게 best-choice 인가

위의 그림에서 가장 먼저 A, B가 RR로 수행되는데, **A, B가 설정한 time slice를 모두 사용했다면 우선순위를 감소**시킨다.
만약, **Time slice를 모두 사용하지 않고 CPU를 포기한다면, 그대로 Q8에 위치**시킨다.
> 이렇게 CPU를 포기하는 작업은, 주로 I/O 작업을 포함한 프로세스이다. 

`MLFQ`는 작업에게 **한 번 우선순위를 설정**하는 것이 아닌, **과거의 실행에 따라 작업의 우선순위를 바꿔준다**.
예를 들어, 어떤 작업이 I/O 작업을 계속 발생한다고 가정하고 MLFQ 스케줄러가 이 작업이 `대화형 프로그램`이라고 판단하면, **우선순위를 높게 설정**한다.
그러다 잠시후,이 작업이 더이상 **I/O를 발생하지 않고 CPU만 계속 사용한다면 MLFQ는 다시 우선순위를 낮게 설정**해준다.

*하지만, 이 방법에도 문제는 존재한다.*

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/1cade332-baef-4513-ae9b-b59d71fe9b46)

A: 엄청 긴 작업
B: 1초마다 I/O를 수행하는 작업

B작업은 10초의 time slice를 수행하기 전에 I/O를 요청하기 때문에, **우선순위가 변하지 않고 계속 최상위에 위치한 상태** 그대로인 것을 볼 수 있다.
이렇게 되면 **2가지의 문제**가 발생할 수 있다.

1. `starvation(기아) 문제`: 어떤 작업이 오랫동안 수행이 안되는 상태
2. 프로세스가 스케줄러를 속이며 계속 우선순위를 높게 유지하는 것 (악용 가능성)

*이를 해결하기 위해 규칙 5가 등장했다.*

### 기아 문제 해결 - 규칙 5. Priority Boost
* 기아 문제를 해결하기 위해 위 규칙을 만들었지만, 프로그램이 수행되던 중 특성이 변하는 문제도 해결할 수 있다. 
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/58d09148-ec8f-4b4b-9405-a4f1ae8524dc)

**A: 100초까지는 CPU를 사용하다가, 100초 이후에는 I/O를 사용하는 작업**

`왼쪽`이 **priority boost를 적용하지 않는 상황**이고, `오른쪽`이 **50초마다 priority boost를 적용한 상황**이다. 

왼쪽 그림에선 A 작업이 100초부터 실행되지 않는 것을 볼 수 있지만, priority boost를 적용한 **오른쪽에선 50초마다 모든 작업의 우선순위가 가장 높은 큐인 Q2로 이동시키며 A도 수행**할 수 있도록 만들어 주는 것을 볼 수 있다.

### gaming of our schedular 문제 해결하기: 규칙 4
> 프로세스가 스케줄러를 속일 수 있는 문제를 gaming of our scheduler라고 한다.

원래 규칙 4는 아래와 같이 두 가지 규칙으로 존재했었다.
* 규칙 4a: 작업이 실행될 때 time slice를 모두 사용하면 우선순위를 감소시킨다.
* 규칙 4b: 작업이 실행될 때 time slice를 모두 사용하지 않고, CPU를 포기하면 우선순위를 그대로 유지한다.

10초의 time slice를 가진 스케줄러에서 의도적으로 9.9초마다 CPU를 포기하는 작업이 들어온다면 해당 작업은 계속해서 높은 우선순위를 유지하게 될 것이다. 

이 문제를 해결하기 위해 규칙 4는 이 둘을 통합하여 만들어졌다.

* 규칙 4 : 작업은 모든 우선순위에서 주어진 time slice를 모두 사용하면 우선순위가 감소한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/3693bff4-67f0-4f18-a1d3-3b18ff814e9e)

위의 그림에서 왼쪽이 기존의 규칙을 적용한 상황이고, 오른쪽이 새로운 규칙을 정의한 상황이다.

왼쪽 그림에서 B가 아주 교묘하게 **time slice보다 적은 시간 동안 실행되다 CPU를 포기하며 우선순위를 유지**하는 것을 볼 수 있다.
따라서, **B가 모두 수행될 때 까지 A 작업은 공평하게 수행될 수 없다.**

규칙 4가 적용된 오른쪽 그림에서는 또 CPU를 time slice보다 적게 사용하고 포기했지만, 이후에 수행되기 전에 사용한 **시간이 누적되었기 때문에 결국 우선순위가 낮아지는 것**을 볼 수 있다.

그러다 결국 가장 우선순위가 낮은 Q0까지 내려와서 A, B는 동일한 우선순위를 가지게된다.

## 참고 자료
https://github.com/NKLCWDT/cs/blob/main/Operating%20System/CPU%20%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81.md
https://baebalja.tistory.com/360
https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/CPU%20Scheduling.md    
