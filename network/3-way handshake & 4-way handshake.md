# 💡 3-way handshake & 4-way handshake

3-Way Handshake 는 TCP의 접속,4-Way Handshake는 TCP의 접속 해제 과정이다.

## 🖇️ Port

- CLOSED
    
    포트가 닫힘.
    
- LISTEN
    
    연결 요청 대기 중.
    
- SYN_RCV
    
    SYNC 요청 받고 응답 대기 중.
    
- ESTABLISHED
    
    포트 연결.
    

## 🖇️ 플래그

CONTROL BIT : 플래그 비트, 6 bit

- URG : 100000
    - URG가 1로 설정된 경우 패킷의 순서에 상관없이 먼저 송신됨.
- **ACK : 010000**
    - 응답 확인, 패킷 받음
    - 최초 연결 설정 제외하고 모든 세그먼트의 ACK 비트는 1로 지정
- PSH : 001000
    - 버퍼링 된 데이터를 가능한 빨리 상위 계층 응용프로그램에 즉시 전달하라는 것을 알리기 위한 컨트롤 비트
- RST : 000100
    - 강제로 연결을 초기화하기 위한 컨트롤 비트
- **SYN : 000010**
    - 연결 설정 → 세션 연결
    - Sequence Number 전송
- **FIN : 000001**
    - 연결 해제 → 세션 연결 종료

## 🔍 3-way handshake

### Concept

네트워크 연결을 설정하는 과정이다.

데이터 전송 전 먼저 정확한 전송을 보장하기 위해서 사전에 세션을 수립하는 과정이다.

### Mechanism

PAR(Positive Acknowledgement with Re-transmission) : 신뢰적인 통신을 제공.

⇒ ACK을 받을 때까지 데이터 유닛 재전송한다.
![mech](https://github.com/hyeong-jun-kim/CS-Study/assets/76768480/8d286534-e467-442a-a2de-41bc36e6a47d)

3개의 segment 교환

⇒ 3-way handshake 매커니즘

### 작동방식
![3-way](https://github.com/hyeong-jun-kim/CS-Study/assets/76768480/8ab1198f-fc61-4553-b3bf-d3582a845c3e)



1️⃣ **SYN**

**클라이언트가 서버와의 연결을 위해 SYN을 보냄. (Seq = x)**

- seq 임의의 랜덤 숫자 지정.
- SYN 플래그 비트 1로 설정

⭐️ Port

- client : `CLOSED` → `SYN_SENT`
- server : `LISTEN`

2️⃣ **SYN + ACK**

**서버가 받았다는 신호로 ACK, SYN 패킷 보냄. (Seq = y, ACK = x+1)**

- 포트 열어달라는 메시지 전송
    
    (SYN-ACK signal bits set)
    
- ACK Number 필드 : Seq + 1로 지정
- SYN, ACK 플래그 비트 1로 설정
- Seq = y, ACK = x+1, SYN, ACK 세그먼트 전송

⭐️ Port

- client : `CLOSED`
- server : `SYN_RCV`

3️⃣ **ACK**

**서버의 응답을 받고 ACK(y+1) 서버로 전송.**

- 수락 확인을 보내 연결을 맺음. (ACK)
- 이 단계에서 데이터 전송 가능.

⭐️ Port

- client : `ESTABLISED`
- server : `SYN_RCV` ⇒ `ACK` ⇒ `ESTABLISHED`

## 🔍 4-way handshake

### Concept

연결을 해제하는 과정이다.

### Termination 종류

1. Graceful connection release : 정상적인 연결 해제
    
    양쪽의 커넥션이 서로 모두 커넥션을 닫을 때가지 연결된다.
    
2. Abrupt connection release : 갑작스런 연결 해제
    - 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
    - 한 사용자가 두 데이터 전송 방향 모두 닫는 경우

### Abrupt 작동방식

RST(TCP Reset) 세그먼트가 전송되면 갑작스러운 연결 해제가 수행된다.

① 존재하지 않는 TCP 연결에 대해 비SYN 세그먼트가 수신된 경우

➁ 열린 커넥션에서 잘못된 헤더가 있는 세그먼트가 수신된 경우

→ RST 세그먼트를 보내 공격을 방지한다.

➂ 기존 TCP 연결을 종료해야 하는 경우

그 외.

- 연결을 지원하는 리소스 부족한 경우
- 원격 호스트에 연결할 수 없고 응답이 중지된 경우

### Graceful 작동방식


> 💡 **Half-Close 기법**<br/>
> 완전히 종료하지 않고 **반만 종료한다.**<br/>
> 🗣️ 종료 요청자가 처음 보내는 FIN 패킷에 승인 번호를 함께 담아서 보내게 되는데, 이때 승인 번호의 의미는 **"일단 연결은 종료할건데 귀는 열어둘게. 이 승인 번호까지 처리했으니까 더 보낼 거 있으면 보내"** 가 된다.<br/>
> 🫂 이후 수신자가 남은 데이터를 모두 보내고 나면 다시 요청자에게 **FIN 패킷**을 보냄으로써 모든 데이터가 처리되었다는 신호 **(3) FIN**를 보낸다. 그럼 요청자는 그때 나머지 반을 닫으면서 안전한 종료가 된다.


![4-way](https://github.com/hyeong-jun-kim/CS-Study/assets/76768480/d7f74a7e-09fc-4195-87bb-7f4da1ecec3c)

**1️⃣ FIN(+ACK) : client → server**

******************클라이언트는 연결된 상태에서 연결 종료를 위해 서버에게 FIN 플래그를 전송.******************

******************************************************************************FIN 플래그에는 실질적으로 ACK 포함.******************************************************************************

- 클라이언트가 `close()` 호출하여 접속을 끊으려고 함.

**2️⃣ ACK : server → client**

**FIN을 받고 확인으로 ACK을 보내고 통신이 끝날 때까지 기다린다.(`TIME_WAIT` 상태)**

**여기서 ACK Number 필드는 seq+1로 지정하고 ACK 플래그 비트를 1로 설정하여 세그먼트 전송.**

- 아직 남은 데이터가 있다면 전송을 마친 후 `close()` 호출 → `CLOSE_WAIT` 상태
- 클라이언트는 ACK을 받은 후 FIN 패킷을 받을 준비를 함 (`FIN_WAIT2`)

**3️⃣ FIN : server → client**

**데이터를 다 보낸 후 FIN 패킷을 클라이언트에게 전송.**

- 승인번호를 보내줄 때까지 기다리는 `LAST_ACK` 상태

**4️⃣ ACK : client → server**

클라이언트는 FIN을 받고 확인했다는 ACK을 전송.

- 서버로부터 못 받은 데이터가 있을 수도 있으므로 `TIME_WAIT` 상태
    
    → 실질적인 `CLOSED` 상태에 들어감.
    
    → `TIME_WAIT` 상태는 의도치 않은 에러로 연결이 데드락에 빠지는 것을 방지. 타임 초과되면 `CLOSED` 에 들어감.
    

**⇒ 서버는 ACK 받은 후 소켓 닫음 (`CLOSED`)**

**⇒ `TIME_WAIT` 끝나면 클라이언트도 닫음 (`CLOSED`)**

---

# 참고

[[네트워크] TCP/UDP와 3 -Way Handshake & 4 -Way Handshake](https://velog.io/@averycode/네트워크-TCPUDP와-3-Way-Handshake4-Way-Handshake)
