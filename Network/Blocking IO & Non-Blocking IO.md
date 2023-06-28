# Blocking & Non-Blocking

> 호출되는 I/O 함수가 바로 리턴되느냐 아니면 제어권을 가져가 Block하느냐의 차이이다.
> 

## Blocking I/O

I/O 작업이 진행되는 동안 유저 프로세스가 자신의 작업을 중단한 채, I/O가 끝날때까지 대기하는 방식.
<img width="563" alt="blocking" src="https://github.com/hyeong-jun-kim/CS-Study/assets/76768480/afc297ea-da30-49b7-9daa-0eb4f0593e1a"><br/>
애플리케이션에서 `Read()`를 호출해 커널에 read I/O를 요청하면, read가 끝날때까지 `block` 되어 다른 작업을 하지 못한다. 

그렇게 되면 I/O작업은 CPU 자원을 거의 쓰지 않아 **Resource 낭비가 심하다.**

**⇒ 비효율적인 동작 방식**

🖥️ 여러 client가 접속하는 서버가 blocking 방식일 경우, 

다른 client가 진행중인 작업을 중지하면 안되므로 별도의 client별로 Thread를 생성해야 한다. **그렇게 되면 접속자수가 매우 많아지고 많아진 Threads로 컨텍스트 스위칭 횟수가 증가한다.**

## Non-Blocking I/O

A 함수가 I/O 작업을 호출했을 때 I/O 작업이 완료될 때까지 A 함수의 작업을 중단하지 않고 I/O 호출에 즉시 리턴하고, A함수가 이어서 다른 일을 수행할 수 있도록 하는 방식.<br/>
<img width="602" alt="non-blocking" src="https://github.com/hyeong-jun-kim/CS-Study/assets/76768480/abba4ce6-759b-4bfa-9886-7526b1e2b0f2">


- EAGAIN : 에러(E)-나중에 다시(AGAIN) 요청
- EWOULDBLOCK : 데이터가 없다는 메시지

**read I/O를 하기 위해 `system call` 을 수행하면, 커널의 I/O 작업 완료와 상관없이 무관하게 즉시 응답한다.**

⇒ 커널이 `system call` 을 받으면 CPU 제어권을 애플리케이션에게 넘김 → 애플리케이션은 I/O 작업이 완료되기 전에 다른 작업을 수행할 수 있음. → 애플리케이션은 다른 작업들을 수행하다가 중간 중간 `system call` 을 보내 I/O가 완료되었는지 커널에게 물어보고, 완료가 되면 I/O 작업을 완료함.

**Blocking 방식에 비해 효율적인 방식**

⇒ 프로세스가 주기적으로 읽기 요청을 보냄. 

 `Busy-Waiting` 방식.

- Context switching을 자주 발생시킴. → 매우 비싼 작업
- 컨디션 검사에 많은 자원을 소모.

# 참고
[blocking I/O, non-blocking I/O에 대하여 (sync, async와의 차이)](https://etloveguitar.tistory.com/140)

[기초지식: Non-blocking I/O, 싱글 스레드, 노드JS의 특징](https://velog.io/@bluecoolgod80/기초지식-Blocking-IO-Non-blocking-IO-SyncAsync-IO-멀티플렉싱)
