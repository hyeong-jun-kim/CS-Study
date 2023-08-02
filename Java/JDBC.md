# JDBC(Java Database Connectivity)

## JDBC란?
Java 기반 애플리케이션의 데이터를 데이터베이스에 저장 및 업데이트하거나, 데이터베이스에 저장된 데이터를 Java에서 사용할 수 있도록(조회) 하는 자바 API

#### 사용 이유
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5aW2q%2FbtrQTVU3vS9%2FQXydPIyxuRzghLBCTVYGnK%2Fimg.png" width=400px/>

어플리케이션 서버와 DB를 연결하기 위해선
1. **커넥션 연결** : 주로 TCP/IP를 사용해 어플리케이션 서버와 DB서버가 연결.
2. **SQL 전달** : 어플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달.
3. **결과 응답** : DB는 전달된 SQL을 수행하고 그 결과를 응답.

의 과정을 거친다.

**"하지만 DB서버를 교체한다면?"**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbp2JF2%2FbtrQVrrY1pE%2FvFQ685paZKQNOkSMBKraTk%2Fimg.png" width=400px/>

예를 들어 MySQL과 Oracle은 커넥션을 연결하는 방법, SQL을 전달하는 방법, 결과를 응답받는 방법이 모두 다르다.

위와 같은 방식처럼 애플리케이션 서버와 DB서버를 직접 연결하는 방식은 데이터베이스를 다른 종류로 변경하면 애플리케이션 서버에서 개발한 데이터베이스 사용 코드를 모두 변경해야 한다.

또한 개발자가 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 결과를 응답받는 방법들을 모두 학습해야 한다는 문제점이 발생한다.


**이러한 문제를 해결할 수 있는게 바로 JDBC라는 자바 표준이다.**

.

## JDBC 표준 인터페이스

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIS7Q7%2FbtrR4G2GJPN%2FU3k0zntzKSMYLO3HJC8431%2Fimg.png" width=500px/>

JDBC는 3가지 기능을 표준 인터페이스로 정의하여 제공한다.

- java.sql.Connection - 연결
- java.sql.Statement - SQL을 담은 내용
- java.sql.ResultSet - SQL 요청 응답

.

## JDBC 동작 흐름

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHk4IH%2FbtrR2EZokg5%2FVlsEZPLukP9YKb7tK0paT1%2Fimg.png" width=500px/>

자바와 DB를 연결하기 위해서는 두 단계를 거쳐야한다.

1. 먼저 JDBC API는 JDK(java.sql)에 포함되어있으며 DBMS에 상관없이 사용할 수 있는 API를 제공한다. JDK에 이미 포함이 되어있기 때문에 따로 다운로드하거나 설치할 필요는 없다.
JDBC API 예시)
    ```
    Connection connection = DriverManager.getConnection(url, user, password);
                Statement statement = connection.createStatement();
                String query = "SELECT * FROM customers";
                ResultSet resultSet = statement.executeQuery(query);
    ```
    - DriverManger : JDBC 드라이버 로드
    - Connectoin : DB와 연결하기 위한 인터페이스
    - Statement : SQl을 보내기 위한 통로. 인자가 없음.
    - PreparedStatement : Statement와 동일한데 차이점은 인자값으로 SQL을 받기 때문에 특정한 SQL에 대한 통로라고 생각하면 된다. 
    - CallableStatement : PL/SQL을 호출할 때 사용
    - ResultSet : SQL문의 결과를 저장하는 객체

2. JDBC 드라이버(.jar)를 통해 DB에 접근한다. JDBC 드라이버는 각 DB 회사에서 자신의 DB에 맞도록 JDBC 인터페이스를 구현해서 라이브러리로 제공한다. 
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd73O6h%2FbtrQTFLIopP%2FyLrBL43RtS02sL3exsbZi1%2Fimg.png" width=500px/>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F787Aj%2FbtrwlKmczJB%2Ff7Cmwmor7eptORg4pY3ktk%2Fimg.png" width=500px/>

#### 요약
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwTyMC%2FbtrR5yww0DA%2Fjwst72s4xtTKhLTtzfJ0mK%2Fimg.png" width=400px/>

.

## 커넥션 풀(Connection Pool)
JDBC API를 사용하여 데이터베이스와 연결하기 위해 Connection 객체를 생성하는 작업은 비용이 많이 드는 작업 중 하나이다.

#### 커넥션 객체를 생성하는 과정
1. 어플리케이션에서 DB 드라이버를 통해 커넥션을 조회한다.
2. DB 드라이버는 DB와 TCP/IP 커넥션을 연결한다. (3 way handshake와 같은 네트워크 연결 동작 발생)
3. DB 드라이버는 TCP/IP 커넥션이 연결되면 아이디와 패스워드, 기타 부가 정보를 DB에 전달한다.
4. DB는 아이디, 패스워드를 통해 내부 인증을 거친 후 내부에 DB를 생성한다.
5. DB는 커넥션 생성이 완료되었다는 응답을 보낸다.
6. DB 드라이버는 커넥션 객체를 생성해서 클라이언트에 반환한다.

**이처럼 커넥션을 새로 만드는 것은 비용이 많이 들며, 비효율적이다.**

이러한 문제를 해결하기 위해 애플리케이션 로딩 시점에 Connection 객체를 미리 생성하고, 애플리케이션에서 데이터베이스에 연결이 필요할 경우 미리 준비된 Connection 객체를 사용하여 애플리케이션의 성능을 향상하는 커넥션 풀(Connection Pool)이 등장하게 된다.

 

Connection 객체를 미리 생성하여 보관하고 어플리케이션이 필요할 때 꺼내서 사용할 수 있도록 관리해 주는 것이 **커넥션 풀**이다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJrVSk%2FbtrRZOhiAim%2FieG4YLJvw4TcH6IvAuk8Ok%2Fimg.png" width=500px/>

- 어플리케이션을 시작하는 시점에 커넥션 풀은 필요한 만큼 커넥션을 미리 생성하여 보관한다.
- 서비스의 특징과 스펙에 따라 생성되는 Connection 객체의 개수는 다르지만 일반적으로 기본값으로 10개를 생성한다.
- 커넥션 풀에 들어있는 Connection 객체는 TCP/IP로 DB와 연결되어 있는 상태이기 때문에 즉시 SQL을 DB에 전달할 수 있다.
- 즉, DB 드라이버를 통해 새로운 커넥션을 획득하는 것이 아닌 이미 생성되어 있는 커넥션을 참조하여 사용하게 된다.
- 커넥션 풀에 있는 커넥션을 요청하면 커넥션 풀은 자신이 가지고 있는 커넥션 객체 중 하나를 반환한다.
 

따라서, DB 드라이버를 통해 커넥션을 조회, 연결, 인증, SQL을 실행하는 시간 등 커넥션 객체를 생성하기 위한 과정을 생략할 수 있게 된다.

Spring Boot 2.0 이전 버전에서는 Apache 재단의 오픈 소스인 Apache Commons DBCP를 주로 사용하였지만, 스프링 부트 2.0 이후 HikariCP를 기본 DBCP로 채택하여 사용되고 있다.

#### 그럼 Connection Pool이 커지면 성능은 무조건 좋아질까?

그렇지 않다. Connection의 주체는 Thread이므로 Thread와 함께 고려해야 한다.
이에 대한 자세한 내용을 알고 싶다면 개별적으로 찾아봐라 ㅋ
## 예상 질문
1. JDBC란?
