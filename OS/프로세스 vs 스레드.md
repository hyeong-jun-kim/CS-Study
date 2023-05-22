# 프로세스 vs 스레드

## 프로그램
* **어떤 작업을 위해 실행할 수 있는 파일** (파일 시스템에 존재하는 실행 파일)
* 프로그램을 실행시키기 전까지는, 그저 코드가 구현되어 있는 파일이다. 

## 프로세스
- 컴퓨터에서 연속적으로 **실행되고 있는 컴퓨터 프로그램**
- 메모리에 올라와 실행되고 있는 프로그램 인스턴스

**특징**
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/bda19568-208d-4200-839d-a0d63c77522e)
- 프로세스는 각각 독립된 영역 (**Code, Data, Stack, Heap**의 영역을 할당받는다.)
- 기본적으로 프로세스당 최소 *1개*의 스레드를 가지고있다.
- 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
- 한 프로세스가 다른 프로세스 자원에 접근하려면, 프로세스 통신을 사용해야한다.


## 스레드
- 프로세스 내에서 실행되는 여러 흐름의 단위

**스레드 구조**
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/1cca6565-1dc8-4f71-9638-c2d863536b23)
- 스레드는 프로세스 내에서 각각 Stack만 할당받고, **Code, Data, Heap의 영역은 공유**한다
- 각각의 스레드는 별도의 레지스터와 스택을 갖고 있지만, 힙 메모리는 서로 읽고 쓸 수 있다.

## 멀티 프로세스와 멀티 스레드의 차이
### 멀티 프로세스
- 하나의 응용 프로그램을 여러 개의 프로세스로 구성하여 각 프로세스가 하나의 작업을 처리하도록 하는 것이다.
* **장점**
    * 여러 개의 자식 프로세스 중 하나에 문제가 발생하면, 그 **자식 프로세스만 죽는 것 이상으로 다른 영향이 확산되지 않는다.**
* **단점**
    * **Context Switching**에서의 오버헤드 
        * Context Switching 과정에서 캐시 메모리 초기화등 무거운 작업이 진행되고, 많은 시간이 소요되는 등의 **오버헤드가 발생**하게 된다.
        > Context Switching이란, 프로세스 상태 정보를 저장하고 복원하는 일련의 과정을 의미한다. 

    * 프로세스 사이의 어렵고 복잡한 통신 기법(IPC)
        * 프로세스는 각각의 독립된 메모리 영역을 할당받았기 때문에, 하나의 프로그램에 속한 프로세스들 사이의 변수를 공유할 수 없다.

### 멀티 스레드
- 하나의 응용 프로그램을 여러 개의 스레드로 구성하고, 각 스레드로 하여금 하나의 작업을 처리하도록 하는 것이다.
* **장점**
    * 독립적인 프로세스에 비해 공유 메모리만큼의 시간, 자원 손실이 감소한다.
    * 전역 변수와 정적 변수에 대한 자료 공유 가능
    * 프로세스간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.
* **단점**
    * 자원 공유의 문제 (동기화 문제)가 발생할 수 있다.
    * 하나의 스레드에 문제가 발생하면, 전체 스레드가 영향을 받는다.

멀티 스레드의 안정성에 대한 단점은 **Critical Section** 기법을 통해 대비할 수 있다.
> 하나의 스레드가 공유 데이터 값을 변경하는 시점에 다른 스레드가 그 값을 읽으려할 때 발생하는 문제를 해결하기 위한 동기화 과정
> ```상호 배제, 진행, 한정된 대기를 충족해야함```

## 쓰레드 풀
`요청마다 쓰레드를 생성하는 경우의 단점을 보완`하고자, 대부분의 **WAS**는 쓰레드 풀을 가지고있다.

WAS의 쓰레드 풀은 다음과 같이 동작한다.
* 풀(Pool)안에 미리 쓰레드를 만들어 놓는다.
* 사용자 요청이 오면 쓰레드 풀에게 사용 안하는 쓰레드를 달라고 요청한다.
* 쓰레드 풀에서 쓰레드를 가져다 쓴다.
* 쓰레드 사용이 끝나면 다시 쓰레드 풀에게 반납을 한다.

만약 쓰레드 풀안에 모든 쓰레드가 사용중인데 쓰레드를 달라는 요청이 오면, `쓰레드 대기`, `거절` 등을 할 수 있다.

**쓰레드 풀의 장점**
- 쓰레드가 미리 생성되어 있으므로, 쓰레드를 생성하고 종료하는 비용이 절약되고, 응답시간이 빠르다.
- 생성가능한 쓰레드의 최대치가 있으므로 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리할 수 있다.

## WAS와 멀티 쓰레드
멀티 쓰레드에 대한 부분은 WAS가 알아서 해준다. `서블릿 컨테이너`는 **동시 요청을 위한 멀티 쓰레드 처리를 지원**하므로, 개발자가 멀티 쓰레드 관련 코드를 신경 쓰지 않으며, 마치 싱글 쓰레드 프로그래밍을 하듯이 코드를 짜면 된다.
그리고 멀티 쓰레드 환경에서 개발을 할 때 `싱글톤 객체`는 주의해서 사용해야 한다. 여기서 코드를 잘못 짜거나 잘못 사용하게되면, `동시성 이슈`가 발생할 수 있다.

## 동시성 이슈
`동시성 이슈`란, **여러 쓰레드가 동시에 같은 인스턴스의 필드 값을 변경하면서 발생하는 문제를 의미**한다.

싱글톤 객체는 `상태 값이 유지되지 않게 설계`를 해야한다.
```
@Service
public class UserService {

    private UserId userId; // 상태 값

    public void createUser(User user) {
        // 여기서 userId 값을 변경하는 코드가 존재
    }
}
```
여기서 **UserService**는 싱글톤 객체로 등록되는 빈이다. 이 코드에서는 userId라는 상태값이 매우 위험하다는 것을 알 수 있다.

그 이유는, 쓰레드는 프로세스의 Stack, Heap, Data, Text 영역 중 `Stack 부분을 제외한 나머지 부분을 공유`하기 때문이다. userId의 상태 값은 전역 변수고 Process의 Data 영역에 적재된다. 이러면 회원 가입후 서로 다른 사람의 개인정보가 보인다는 등의 심각한 오류가 발생한다.

따라서, 동시성 이슈를 피하기 위해서는 쓰레드의 Stack 영역이 **공유하지 않는 특성**을 생각하며, 전역 변수대신 `지역 변수`를 쓰게끔 하면 된다.

<details>
<summary>
(참고) 자바로 싱글톤 패턴을 구현하는 7가지 방법
</summary>
    
### 1. 단순한 메서드 호출

- **문제점:** 원자성이 결여되어있다. ⇒ 자바에서는 멀티 스레드로 동작하므로, 문제 발생할 수 있음
    - 여러 스레드에서 동시에 인스턴스를 생성해서 여러개의 인스턴스가 생성될 수 있다.
- **해결방법:** synchronized 키워드를 붙혀서 해당 문제점을 해결할 수 있다.
    - 하지만 락이 걸려있으므로, 성능저하 이슈가 있을 수 있다.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();

        return instance;
    }
}
```

## 2. 정적 멤버

- `정적(static)` 멤버나 블록은 런타임이 아니라, **최초에 JVM이 클래스 로딩 때 모든 클래스들을 로드할 때 미리 인스턴스를 생성**하는데 이를 이용한 방법이다.
- 클래스 로딩과 동시에 싱글톤 인스턴스를 만들고, 그렇기 때문에 모듈들이 싱글톤 인스턴스를 요청할 때 그냥 만들어진 인스턴스를 반환하면 된다.
- **문제점:** 불필요한 자원 낭비라는 이슈가 있다. 싱글톤 인스턴스가 필요없을 경우에도 무조건 싱글톤 클래스를 호출해 인스턴스를 만들어야 하기 때문이다.

```java
public class Singleton {
    private final static Singleton instance = new Singleton();

    private Singleton() {

    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

## 3. 정적 블록

```java
public class Singleton {
    private static Singleton instance = null;

    static {
        instance = new Singleton();
    }

    private Singleton() {

    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

## 4. 정적 멤버와 LazyHolder(중첩 클래스)

- **singleInstaceHolder**라는 내부클래스를 하나 더 만듬으로써, Singleton 클래스가 최초에 로딩되더라도 함께 초기화가 되지 않고, getInstance()가 호출될 때 singleInstanceHolder 클래스가 로딩되어 인스턴스를 생성하게 된다.

```java
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}
```

## 5. 이중 확인 잠금(DCL)

- 이중 확인 잠금은 **인스턴스 생성 여부를 싱글톤 패턴 잠금 전에 한번, 객체를 생성하기 전에 한번 2번 체크**하면 인스턴스가 존재하지 않을 때만 잠금을 걸 수 있기 때문에 앞서 생겼던 문제점들을 해결할 수 있다.

```java
public class Singleton {
    private volatile Singleton instance;

    private Singleton() {

    }

    public Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null)
                    instance = new Singleton();
            }
        }
    }
}
```

## 6. enum

- enum 클래스는 기본적으로 tread safe한 점이 보장되기 때문에, 이를 통해 생성할 수 있다.

```jsx
    public enum SingletonEnum {
        INSTANCE;
        public void oortCloud() {

        }
    }
```

## 추천하는 방법은

1. 제일 많이 쓰이는 4번(정적 멤버와 LazyHolder)
2. 6번은 이펙티브 자바를 쓴 조슈아 블로크가 추천한 방법이다.
</details>
    
## Thread Safe하게 설계하는 방법
Thread Safe는 동시성 이슈가 발생하지 않도록 설계하는 것을 말하고, OS 지식과 풀어서 설명하면, 쓰레드가 경쟁 상태(race condition)일 때 임계 구역에 있는 공유 변수의 일관성을 유지시키도록 설계하는 것을 말한다.
* java.util.concurrent 패키지 하위 클래스 사용하기
    * Ex) ConcurrentHashMap 등
* Singleton 패턴을 사용
    * 보통 LazyHolder, Enum 방식을 사용한다
        * LazyHolder는 Class loader 매커니즘을 이용한 방법
* 동기화 블럭(synchronized) 지정

### 참고 자료
* https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/Process%20vs%20Thread.md
* https://github.com/NKLCWDT/cs/blob/main/Operating%20System/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80%20%EC%93%B0%EB%A0%88%EB%93%9C.md
* https://www.youtube.com/watch?v=3rfbnQYOCFA
* https://www.youtube.com/watch?v=1grtWKqTn50&t=483s
