# 로드 밸런싱(Load Balancing)

## 로드 밸런싱이란?
> 네트워크 또는 서버에 가해지는 부하를 분산 해주는 기술

중앙처리장치 혹은 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미

#### 로드 밸런싱의 필요성
서비스의 제공 초기 단계라면 적은 수의 클라이언트로 인해 서버 한 대로 요청에 응답하는 것이 가능하다. 하지만 사업의 규모가 확장되고, 클라이언트의 수가 늘어나게 되면 기존 서버만으로는 정상적인 서비스가 불가능하게 된다. 증가한 트래픽에 대처할 수 있는 방법은 크게 2가지이다.

1) `Scale-up` : 서버 자체의 성능을 확장하는 것. 비유하자면 CPU가 i3인 컴퓨터를 i7으로 업그레이드하는 것과 같다.
2) `Scale-out`: 기존 서버와 동일하거나 낮은 성능의 서버를 두 대 이상 증설하여 운영하는 것을 의미한다. CPU가 i3인 컴퓨터를 여러 대 추가 구입해 운영하 것에 비유할 수 있다.

**Scale-out의 경우, 여러 대의 서버로 트래픽을 균등하게 분산해주는 로드밸런싱이 반드시 필요하다.**

.

## 로드 밸런서란?
> 로드밸런싱 기술을 제공하는 서비스 또는 장치

클라이언트와 네트워크 트래픽이 집중되는 서버들 또는 네트워크 허브 사이에 위치한다.

.

### 로드 밸런서 종류

#### 1. L4 로드 밸런서
> 전송 계층에서 부하를 분산한다.

IP주소나 포트번호, MAC주소 등에 따라 트래픽을 나누고 분산처리가 가능하다.

예를 들어, 로드 밸런서에 들어오는 요청이 [`213.12.32.123:80, 213.12.32.123:1024`]으로 들어온다면, L4 로드 밸런서는 해당 요청을 서버 풀에 있는 여러 대상 서버 중 하나로 전달한다. 이는 TCP/UDP 패킷을 확인하여 로드 밸런싱하는 것으로, **응용 계층의 내용은 신경쓰지 않는다.**

#### 2. L7 로드 밸런서
> 애플리케이션 계층에서 부하를 분산한다.

OSI 7계층의 프로토콜(HTTP, SMTP, FTP 등)을 바탕으로도 분산 처리가 가능하다.

예를 들어, 웹 서비스의 경우 HTTP 요청 헤더, URL, 쿠키 등을 확인하여 특정 조건에 따라 요청을 서버에 분산시킨다.

사용자가 '/products' 경로로 요청을 보낸다면, L7 로드 밸런서는 해당 요청을 처리할 수 있는 서버로 전달한다. 또한, L7 로드 밸런서는 세션 유지, SSL 종료, 캐싱 등과 같은 고급 기능도 제공할 수 있다.


<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtBUtz%2Fbtq6PDcX88h%2FzB9Ka82SOD8bazHlX79YRK%2Fimg.png" weight=400px height=800px>

### 각각의 장단점
<img src="https://velog.velcdn.com/images%2Fmakeitcloud%2Fpost%2F76db786e-1e41-4d91-aff7-9d3a5f6cde42%2Fimage.png" height=500px>

.

## 로드밸런싱 알고리즘

#### 1. 라운드로빈 방식(Round Robin Method)
- 서버에 들어온 요청을 순서대로 돌아가며 배정하는 방식
- 클라이언트의 요청을 순서대로 분배하기 때문에 여러 대의 서버가 동일한 스펙을 갖고 있고, 서버와의 연결(세션)이 오래 지속되지 않는 경우에 활용하기 적합함.

#### 2. 가중 라운드로빈 방식(Weighted Round Robin Method)
- 각각의 서버마다 가중치를 매기고 가중치가 높은 서버에 클라이언트 요청을 우선적으로 배분하는 방식
- 주로 서버의 트래픽 처리 능력이 상이한 경우 사용되는 부하 분산 방식이다. 예를 들어 A라는 서버가 5라는 가중치를 갖고 B라는 서버가 2라는 가중치를 갖는다면, 로드밸런서는 라운드로빈 방식으로 A 서버에 5개 B 서버에 2개의 요청을 전달함.

#### 3. IP 해시 방식(IP Hash Method)
- 클라이언트의 IP 주소를 특정 서버로 매핑하여 요청을 처리하는 방식
- 사용자의 IP를 해싱해(Hashing, 임의의 길이를 지닌 데이터를 고정된 길이의 데이터로 매핑하는 것, 또는 그러한 함수) 로드를 분배하기 때문에 사용자가 항상 동일한 서버로 연결되는 것을 보장함.

#### 4. 최소 연결 방식(Least Connection Method)
- 요청이 들어온 시점에 가장 적은 연결상태를 보이는 서버에 우선적으로 트래픽을 배분하는 방식
- 자주 세션이 길어지거나, 서버에 분배된 트래픽들이 일정하지 않은 경우에 적합함.

#### 5. 최소 리스폰타임(Least Response Time Method)
- 서버의 현재 연결 상태와 응답시간(Response Time, 서버에 요청을 보내고 최초 응답을 받을 때까지 소요되는 시간)을 모두 고려하여 트래픽을 배분하는 방식
- 가장 적은 연결 상태와 가장 짧은 응답시간을 보이는 서버에 우선적으로 로드를 배분함.
