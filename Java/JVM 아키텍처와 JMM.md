# JVM (Java Virtual Machine)
- 자바 프로그램이 플랫폼에 의존하지 않고, 어디서든 동작 가능하도록 하기 위한 Java 가상 머신이다.
- C, C++ 언어는 CPU 아키텍처, 운영체제 등 플랫폼 환경에 의존성을 가지기 때문에 플랫폼이 바뀌면 제대로 동작하지 않는 문제가 있다.

![image](https://user-images.githubusercontent.com/70622731/160238400-cac9e118-138a-405d-81ee-feed44c3cb9a.png)

### JVM의 특징
**스택 기반의 가상머신**
- 인텔 x86, ARM 아키텍처는 하드웨어가 레지스터 기반으로 동작하는 반면에, JVM은 스택 기반으로 동작한다.

**심볼릭 레퍼런스**
> 참고하는 클래스의 특정 메모레 주소를 참조관계로 구성한 것이 아니라, 참조하는 대상의 이름만을 지정한 것이다. Class 파일이 JVM에 올라가게 되면, 심볼릭 레퍼런스는 그 이름에 맞는 객체 주소를 찾아서 연결하는 작업을 수행한다. 그러므로 실제 메모리 주소가 아니라 이름만을 가진다.
- 기본 자료형을 제외한 모든 타입(클래스와 인터페이스)를 명시적인 메모리 기반의 레퍼런스가 아니라 심볼릭 레퍼런스를 통해 참조한다.

**가비지 컬렉션**
- 클래스 인스턴스는 사용자 코드에 의해 명시적으로 생성되고, 가비지 컬렉션에 의해 자동으로 파괴된다.

**기본 자료형을 명확하게 정의하여 플랫폼 독립성 보장**
- C/C++등의 전통적인 언어는 플랫폼에 따라 int의 크기가 변한다, JVM은 기본 자료형을 명확하게 정의해 호환성을 유지하고 플랫폼 독립성을 보장한다.

### JDK (Java Development Kit), JRE(Java Runtime Environment)
![image](https://user-images.githubusercontent.com/70622731/149750883-787d64f6-84db-4bd0-aff6-620825089de3.png)

JVM, JRE, JDK는 자바 프로그래밍의 기술 패키지로 불린다.
JDK는 자바 기반의 소프트웨어를 개발하기 위한 도구이며, JRE는 자바 코드를 실행하기 위한 환경으로 JVM을 포함하고 있다. JDK는 JRE을 가지고 있고, 컴파일러도 가지고있다.

### JDK 아키텍처
![image](https://user-images.githubusercontent.com/70622731/149751223-bc8f19b7-c64b-4681-b8e0-af739582cbd0.png)

클래스로더가 컴파일된 자바 바이트 코드를 메모리영역에 로드하고, 실행엔진이 자바 바이트코드를 실행한다.
> 바이트 코드 (byte code)
> 특정 하드웨어가 아닌 가상 컴퓨터(VM)에서 돌아가는 실행 프로그램을 위한 이진 표현법으로 하드웨어가 아닌 소프트웨어에 의해 처리되기 때문에 기계어보다 추상적이다.

### 1. Class Loader
자바 바이트 코드(.class)를 Runtime Momery Area에 적재하는 역할을 함

자바는 런타임에 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크하는 특징이 있다.

이 동적 로드를 담당하는 부분이 JVM의 클래스 로더이다.

클래스 로더가 아직 로드되지 않는 클래스를 찾으면, 다음 그림과 같은 과정을 거쳐 클래스를 로드하고, 링크하고 초기화한다.

![image](https://user-images.githubusercontent.com/70622731/149752948-2df80575-4249-44d9-b635-e447a6c68384.png)

- 로딩 : 클래스를 읽어오는 과정
- 링크 : 레퍼런스 연결과정
- 초기화 : static 값 초기화하고 변수 할당하는 과정

**1) Loading**
- 클래스 로더가 .class 파일을 읽고, 내용에 맞는 binary 데이터를 생성한 뒤 메모리의 Method 영역에 저장한다.

클래스 로더의 종류로는 세가지가 있다.
- **Application Class Loader**
    - 애플리케이션 class path에서 클래스를 읽는다.
- **Extention Class Loader**
    - JAVA_HOME\lib\ext 폴더 또는 java.ext.dirs 시스템 변수에 해당하는 위치에 있는 클래스를 읽는다.
- **Bootstrap ClassLoader**
    - JAVA_HOME\lib 에 있는 코어 자바 API를 제공한다.

![image](https://user-images.githubusercontent.com/70622731/149753059-4f6edbaf-8178-4197-a455-a94851f8b000.png)

클래스를 찾을 때 Application 부터 Bootstrap 순서로 위임하듯이 클래스를 찾게된다.

**2) Linking**
- **검증 (Verifying)**
    - 읽어들인 클래스가 자바 언어 명세 및 JVM 명세에 명시된대로 잘 구성되어 있는지 검사한다.
- **준비 (Preparing)**
    - 클래스가 필요로하는 메모리를 할당하고, 클래스에서 정의된 필드, 메서드, 인터페이스들을 나타내는 데이터 구조를 준비한다.
- **분석 (Resolving)**
    - 클래스의 상수 풀 내의 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경한다.
    - `Book book = new Book()` 이라는 코드가 있을 때, book이라는 참조 변수가 Heap에 저장된 실제 Book 클래스를 가리킬 수 있도록 연결하는게 Resolve 과정이다.

**3) Initiailzing**

클래스 변수들을 적절한 값으로 초기화 한다. 즉, static initializer들을 수행하고, static 필드들을 설정된 값으로 초기화한다.

### 2. Runtime Memory Area
JVM 메모리를 Runtime Memory Area라고 부른다.

**1) Method Area**
- Class, Code, Static Area 등으로 불리는 해당 영역은 코드에서 사용되는 클래스 파일을 클래스 로더로 읽어 클래스별로 런타임 상수풀, 필드데이터, 메소드 데이터, 메소드 코드, 생성자 코드 등을 분류해서 저장한다.
- 모든 스레드가 공유하는 공간
- JVM이 실행될 때 생성됨

> **런타임 상수풀**은 상수를 저장하는 공간이다. 이외에도 필드나 메소드등의 Reference 값들을 저장하고 있고, 실행중에 중복되는 정보가 필요할 때 기존의 정보를 사용하도록 도와준다.

**2) Heap Area**
- new 연산자를 통해 동적으로 생성되는 객체가 저장되는 공간
- Heap에 저장된 데이터는 메모리 관리가 필요한 GC 대상
- 모든 스레드가 공유하는 공간
- JVM이 실행될 때 생성됨

**3) Stack**
- 프레임(Frame)이 저장되는 공간
    - Frame: 메소드 데이터가 저장되는 공간(메소드 파라미터, 지역변수, 참조 주소값 등)
- 다른 스레드와 공유하지 않고 각각의 스레드마다 가지는 공간
- 프레임은 메소드가 실행될 때 Stack에 push하여 추가된다.
- 반대로, 메소드가 종료되면 Stack에서 pop 되어 제거된다.

**4) PC Register**
- 스레드가 생성될 때마다 생성되는 영역으로, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. 이것을 이용해 쓰레드를 돌아가며 수행할 수 있게 한다.

**5) Native Method Stack**
- 자바 스레드와 네이티브 코드로 작성된 코드 사이를 매핑하는 역할을 한다.
- 이러한 라이브러리는 레거시 데이터 혹은 성능을 위해서 사용되었는데 자바의 라이브러리도 발전을 하면서 점차 쓰이지 않게 되었다.
- 이러한 네이티브 코드는 프로그래머가 JNI(Java Native Interface)를 통해서 호출할 수 있다.

### 3. Excution Engine
클래스 로더에 의해 메모리에 적재된 클래스 (ByteCodes)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다.

해당 엔진이 실행하는 두 가지 방식이 존재한다.

**1) Interpreter**
- 인터프리터 방식은 바이트코드를 한줄씩 해석하고 실행한다. 속도가 느리다는 단점이 있다.

**2) JIT(Just In TIme)**
- 기존 인터프리터 방식의 느리다는 단점을 극복하기 위한 방법
- 컴파일 방식과 인터프리터 방식을 혼합한 방식을 이용함

그리고 더이상 참조되지 않는 객체를 모아서 정리하는 GC(Garabage Collector)가 있다.

### JMM (Java Memory Model)
JVM은 다른 소프트웨어와 마찬가지로 호스트 OS 메모리에서 사용가능한 공간을 사용한다.

그러나 JVM 내부에는 런타임 데이터와 컴파일된 코드를 저장하기 위해 별도의 메모리 공간(Heap, Non-Heap, Cache)이 존재한다.

### 1. Heap Memory
기본적으로 Heap Memory는 JVM이 시작될 때 공간을 할당받고, 애플리케이션이 작동 중일 때 사이즈에 있어 자유롭다.
즉, new 연산자를 활용해서 객체를 생성할 때 할당된다.

![image](https://user-images.githubusercontent.com/70622731/160238436-ecf28246-1045-4a0f-bd86-8e3e544a0dde.png)

**1) Young Generation**

Young Generation은 비교적 최근에 할당된 객체들을 가지고있다.

- 새로 생성된 객체는 Eden 공간으로 가고, Eden 공간이 객체들로 가득 찼을때는 minor GC가 수행되며, 살아남은 객체들은 Survivor 공간으로 이동한다.
- Minor GC는 살아남은 객체들을 확인하고, 이들을 survivor 공간으로 이동시키는데, 이 때 한개의 survivor 공간은 항상 비어있어야 한다.
    - 여기서 말하는 survivor 객체는 계속해서 참조가 되고있는 객체들이다.

**2) Old Generation (= Tenured space)**

Old Generation은 Minor GC로부터 살아남고, 오랫동안 사용될 예정의 객체들이다. 
만약 Old Generation마저 가득 차게된다면, Major GC가 작동해서 최근에 사용되지 않는 객체들로부터 가지고 간다.

### 2. Non - Heap Memory (힙이 아닌 영역)
![image](https://user-images.githubusercontent.com/70622731/160238448-2730498f-deaa-4df4-8ff5-ee74a06f3479.png)

- 힙이 아닌 영역은, 모든 스레드가 공유하는 메서드 영역이다.
- Non-Heap 영역은 Permanent Generation을 포함하고 있다. JRE에 포함되어 있지만, Heap 영역과 별도로 메모리에 존재한다.

![image](https://user-images.githubusercontent.com/70622731/160238471-eb6c63b6-c759-4293-839e-ff293cf72b7f.png)

### 3. Cache memory
컴파일러에 의해 컴파일된 기계어가 저장된다.

## 참고자료
https://github.com/NKLCWDT/cs/blob/main/Java/JVM%20%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EC%99%80%20JMM.md