# TCP (흐름제어/혼잡제어)
## Overview
### TCP
- Stream-of-bytes service
    - segment 단위로 조각을 내서 보내는 방식
- 신뢰성, 패킷의 순서를 보장함
    - 에러가 있으면, 이 에러에 대해서 재전송을 함
        - checksum 존재
    - 순서에 잘 맞춰서 갔는지 확인 함
- Full-duplex
    - A->B가 요청을 해서 커넥션을 맺으면, B->A도 데이터를 보낼 수 있다. (양방향)
- connection-oriented
    - TCP Connection을 맺음 (3-way-handshake)
- flow-control
    - receiver가 문제를 일으킬 수 있다는 관점
- congestion control
    - 라우터에 데이터가 들어갈 수 있는 양보다 나갈 수 있는 양이 많아지면, 큐잉딜레이가 증가하고 버퍼가 꽉차버리면 loss가 발생할 수 있다. 이런 현상을 congestion이라 한다.

### Cumulative ACK
![image](https://github.com/lsh9295/CS-Study/assets/53989167/d5d8a989-454b-4774-a04c-df149fc93782)
- 누적 정보를 담은 ACK를 의미한다.
- ACK의 손실이 났을 때, 효과를 볼 수 있다.
- 여기서는 30번 ACK이 loss가 발생했는데, sender 입장에서는 data가 없어진건지, ACK가 없어진건지 구분이 안된다.
- Cumulative ACK를 사용하면 다시 전송할 필요없이, 현재까지 받은 segment 기준으로 ACK를 받으면 된다

### TCP Retransmission
![image](https://github.com/lsh9295/CS-Study/assets/53989167/f990d10c-24e2-48bb-ae8a-470c57afb9ad)
- 이 경우에서 sender 입장에서는 timeout이 발생한다.
- ACK가 loss가 발생했을 경우에는, 다시 100번 ACK를 보내준다.

![image](https://github.com/lsh9295/CS-Study/assets/53989167/ba8829c8-4752-464e-89e0-c6858730a2fc)
- 중간에 segment loss가 발생해서 ACK가 계속 100을 호출하는 상황
    - 이 현상을 **duplicated ACK**라고 한다.
- 맨 마지막에는 ACK 140을 호출하게 된다.
- sender 입장에서는 duplicated ACK를 받게되면 loss가 났다고 판단할 수 있다.

## Congestion Control
네트워크 내의 패킷 수를 조절하여 **네트워크 오버플로우를 방지**하는 기능이다.
- 데이터의 양이 라우터가 처리할 수 있는 양을 초과하면 초과된 데이터는 라우터가 처리하지 못한다.
- 이때 송신 측에는 라우터가 처리하지 못한 데이터를 손실 데이터로 간주하고 계속 재전송하여 네트워크를 혼잡하게 한다.
- 이런 상황은 송신 측의 전송 속도를 적절히 조절해 예방할 수 있는데, 이것을 혼잡 제어라고 한다.

### 혼잡 제어 기법
**1. AIMD (Additive Increase/Multicative Decrease)**
- 처음에 패킷을 **하나씩** 보내고 문제없이 도착하면 윈도우 크기를 **1씩 증가**시켜가며 전송한다.
- 만약 전송에 실패하면, **윈도우 크기를 반으로 줄인다.**
- 윈도우 크기를 너무 조금씩 늘리기 때문에 네트워크의 모든 대역을 활용하여 제대로 된 속도로 통신하기까지 시간이 오래 걸린다는 단점이 있다

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/442b3b9e-f7d5-4514-acb7-9caf3854193e)

**2. Slow Start**
- 윈도우의 크기를 1, 2, 4, 8, ...와 같이 **지수적으로 증가**시키다가 혼잡이 감지되면 **윈도우 크기를 1로 줄이는** 방식이다.
- 보낸 데이터의 ACK가 도착할 때마다 윈도우 크기를 증가시키기 때문에 처음에는 윈도우 크기가 조금 느리게 증가할지라도, 시간이 가면 윈도우 크기가 점점 빠르게 증가한다는 장점이 있다.
- Time-out 상황이 발생했을 때 해당 방법을 적용한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/c4ea3f10-cbc6-4e77-a277-582e19e2f715)

**3. TCP fast retransmit**
- 여러개의 데이터를 보내는 경우가 많기 때문에, 중간에 segment가 손실이 나면 duplicate ACK가 온다.
- 3개의 duplicated ACK을 받게되면 retransmission을 실시한다.

**4. Fast Recovery**
- 혼잡한 상태가 되면 윈도우 크기를 1로 줄이지 않고 반으로 줄이고, 선형으로 증가시키는 방법
- 반으로 줄이고 AIMD 방식처럼 동작

## Flow-Control
**송수신 측 사이의 패킷 수를 제어**하는 기능이다.

- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 문제가 빠를 경우 문제가 생긴다.
- 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번히 발생한다.
- 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야 한다.

### 해결 방법
1. **Stop and Wait**
    - 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법, 하지만 단점으로는 실제 capacity보다 현저히 낮은 패킷을 보낼수 있음

    ![image](https://github.com/lsh9295/CS-Study/assets/53989167/9e043716-d0eb-47bd-85b7-03e86996252a)

2. **Sliding window**
    - segment 단위로 조각을 내서 보내게 됨
    - window의 크기가 클수록 많은 정보를 받을 수 있음
    - 하지만 제약사항이 존재한다, 왜냐면 TCP의 receiver와 router가 overflow가 발생할 수 있기 때문이다.

**특징**
1. **흐름제어 기법** - Transport Layer 제공 흐름제어 기법
2. **연속 전송** - 응답을 기다리지 않고 연속 패킷 전송
3. **크기 동적변환** - 윈도우 크기가 상황에 맞게 동적으로 변화

**동작 방식**

먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷을 전송

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/73edcfc9-28fc-477b-9cc1-b6152ddf0a4d)
    
**구성**
- **윈도우 크기** - 전송했으나 확인 응답 받지 못한 데이터와 지연없이 전송 가능 데이터 합계
- **송신버퍼 크기** - 수신 측의 여유 버퍼 공간을 반영하여 동적으로 변경 

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/bd6ba8f3-d188-40c4-b817-b2f301729da3)

**알고리즘**
- **윈도우 열림동작**
    - 수신측 ACK 도착, 윈도우 우측 경계 오른쪽 이동
    - 늘어난만큼 더 많은 데이터의 전송 가능
- **윈도우 닫힘동작**
    - 데이터 전송, 윈도우 좌측 경계 오른쪽 이동
    - 전송측은 이 데이터에 관여할 필요 없음
- **윈도우 크기결정**
    - 수신측 윈도우 혼잡 윈도우 크기 중 작은 값 
    - ACK 포함 세그먼트를 사용하여 상대방에게 알림
    - 혼잡상태가 발생하지 않도록 네트워크에서 결정한 값

즉, 수신 프로세스 처리 속도에 송신 윈도우 크기가 비례하며, 데이터 송수신에 대한 흐름 제어를 수행한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/e4892a58-cdfe-4d35-9ac4-e121fc7a0779)

## 참고자료
1. http://blog.skby.net/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%94%A9-%EC%9C%88%EB%8F%84%EC%9A%B0sliding-window
2. https://steady-coding.tistory.com/507
3. https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%20(%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4).md#tcp-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4