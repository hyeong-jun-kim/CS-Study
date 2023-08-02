# 컬렉션 프레임워크(Collection Framework)
> 컬렉션은 다수의 데이터, 프레임워크는 표준화된 프로그래밍 방식.
따라서 컬렉션 프레임워크란 데이터 그룹을 저장하는 클래스들을 표준화한 설계를 의미.

- 배열을 사용하다 보면 여러가지 비효율적인 문제가 발생한다.
- 가장 큰 문제점은 크기가 고정적이라는 것.
- 배열의 크기는 생성할 때 결정되며 그 크기를 넘어가게 되면 더이상 데이터를 저장할 수 없다.
- 또한 데이터를 삭제하면 해당 인덱스의 데이터는 비어있어 메모리가 낭비되는 등 여러 문제점들이 발생한다.
- 그렇기에 자바는 배열의 이러한 문제점을 해결하기 위해, 자료구조를 바탕으로 객체나 데이터들을 효율적으로 관리할 수 있는 자료구조들을 제공한다.
- 이러한 자료구조들이 있는 라이브러리를 컬렉션 프레임워크라고 한다.
- 대표적으로는 List, Set, Map, Stack, Queue 등이 존재.

<img src="https://hudi.blog/static/1bacac1babc556100455a8c64e7658da/e6c4b/2.png" width=600px />

.

## Collection 인터페이스
아래는 대표적인 Collection 공통 메서드들이다.

- boolean add(E e) : 해당 컬렉션에 전달된 요소를 추가
- boolean remove(Object o) : 해당 컬렉션에서 전달된 객체를 제거
- void clear() : 해당 컬렉션의 모든 요소를 제거
- boolean contains(Object o) : 해당 컬렉션이 전달된 객체를 포함하고 있는지
- boolean equals(Object o) : 해당 컬렉션과 전달된 객체가 같은지
- boolean isEmpty() : 해당 컬렉션이 비어있는지
- Iterator < E> iterator() : 해당 컬렉션의 반복자(iterator)를 반환
- int size() : 해당 컬렉션의 요소의 총개수를 반환
- Object [] toArray() : 해당 컬렉션의 모든 요소를 Object 타입의 배열로 반환

.
    
## List (순서 O, 중복 O)
> Collection을 상속받는 인터페이스

```
List<String> list = new ArrayList<>();
List<String> list = new LinkedList<>();
```

- 선형 자료구조를 가지고 있어  데이터가 연속적/순차적으로 저장된다.
- 연속적으로 객체를 저장하기 때문에 배열과 마찬가지로 "인덱스"를 통해 객체를 추가/수정/삭제/검색.
- 또한 중복을 허용하기 때문에, 같은 값을 저장할 수 있다.

#### 하위 클래스
- ArrayList 
- LinkedList
- Vector
- Stack

### !! Stack과 Vector의 사용을 지양하자 !!
자바도 Vector와 Stack이라는 컬렉션 프레임워크를 가지고 있다. 그렇다면 자바에서도 Vector와 Stack이 서로의 장점을 살려 여러 용도로 쓰이고 있을까?

**아니다. 아직 사용하는 사람들이 있어 Deprecated 되지만 않았을 뿐, 사실 둘 다 가급적 쓰여서는 안되는 컬렉션이다.**

Vector의 구현방법에 그 이유가 있다.

- Vector는 get()과 set()역할을 하는 모든 메서드에 synchronized 키워드가 붙어 있다.
- Vector의 모든 get() set() 등의 메서드에 synchronized가 붙어있는건 특정 상황에서 성능을 꽤 저하시킬 수 있다.
- 단순히 Vector에 Iterator를 붙여 순차적으로 item들을 탐색하기만 해도 원소탐색 시마다 get() 메서드의 실행을 위해 계속 lock을 걸고 닫으므로 Iterator연산과정 전체에 1번만 걸어주면 될 locking에 쓸데없는 오버헤드가 엄청나게 발생한다.
- 따라서 멀티스레드 프로그래밍을 하는게 아니라면, 비슷한 역할을 하는 ArrayList를 사용하는게 좋다. (ArrayList에는 synchronized 키워드가 전혀 없다.)
- 따라서 Vector는 특정 상황에서만 최적으로 동작하게 되고, 어떤 상황에서는 그렇지 않게 되므로 효율적인 Thread-safe 컬렉션이라고 할 수 없는 것이다.

.

그럼 Stack의 경우도 살펴보자.

Stack에는 Vector처럼 get() set() 메서드가 있는 것도 아닌데 왜 그런걸까?

**Stack이 Vector를 상속받기 때문이다.**

- Vector를 상속받게 된 Stack은 Vector의 기능을 전부 사용할 수 있게된다.
- 따라서 C++의 경우처럼 Stack STL이 완벽한 LIFO 구조를 가질 수도 없게되고, 앞서 말한 Vector의 모든 단점을 그대로 물려받게 되는 것이다.

그렇다면 Stack은 어떤 컬렉션 프레임워크로 대체되는게 좋은가?

일반적인 환경에서는 Deque을 가장 많이 쓴다.
- 자바에서 Deque를 구현하는 방법은 크게 LinkedList와 ArrayDeque가 있는데, 스택의 size가 엄청 커질 가능성이 있고 size의 변동성이 매우 큰 경우 즉각적인 메모리 공간 확보를 위해선 LinkedList방식이 적절하고, 그렇지 않은 경우는 자체 메모리 소모량이 적고 iterate의 효율이 좋은 ArrayDeque를 사용하면 된다.
- 그 외 특수한 상황을 더 고려해보자면, 멀티스레드 환경에서는 ConcurrentLinkedDeque를 사용하는게 좋고, 스택의 size가 제한되어있는 경우에 LinkedBlockingDeque를 사용하는 것을 고려해볼만 하다.

#### 결론
1. Java의 Vector와 Stack은 멀티스레드 환경의 여부와 상관없이 대부분의 조건에서 성능 저하를 일으킨다.
2. Vector가 필요한 상황이라면 대신 ArrayList를 사용하는 것이 바람직하다.
3. 마찬가지로 Stack 대신 Deque의 하위컬렉션을 상황에 맞게, 혹은 그냥 ArrayList를 사용하는 것이 적절하다.

.

## Set (순서 X, 중복 X)
> Collection을 상속받는 인터페이스

`HashSet<String> hashSet = new HashSet<>();`

주된 용도는 중복되는 것을 방지하고, 원하는 값이 포함되어 있는지 확인하기 위함
순서가 없기 때문에 인덱스로 접근이 불가하며 객체에 접근하기 위해서는 iterator 을 사용해야 한다.

#### 하위 클래스
- HashSet
    - Hashing을 이용해서 구현한 컬렉션.
- TreeSet
    - 이진탐색트리(Red-Black Tree)의 형태로 데이터를 저장한다.
데이터 추가, 삭제에는 시간이 더 걸리지만, 검색과 정렬이 더 뛰어나다.
기본적으로 오름차순으로 데이터를 정렬한다.
- LinkedHashSet
    - 	HashSet 클래스를 상속받은 LinkedList로,
데이터에 삽입된 순서대로 데이터를 관리한다.

.

## Map (순서X, 중복 키 X / 중복 데이터 O) 
> Collection을 상속받지 않는 자료구조

<img src="https://velog.velcdn.com/images/sweet_sumin/post/126632d7-2ca4-47bd-84dd-af08301cde63/image.png" width=500px/>

`HashMap<String , String> map = new HashMap<String , String>();`

Map은 컬렉션 프레임워크에서 <키(Key),값(Value)> 형식을 가지고 있는 인터페이스.

#### 하위 클래스
- HashMap
- HashTable
- TreeMap
- LinkedHashMap

키 중복, 같은 키의 여러 value의 경우 **Multimap** 사용

정렬이 필요한 경우 **TreeMap** 사용 (검색에 있어서 가장 느리다.)

메모리 보다 성능이 우선일 경우 **HashMap** 사용 (검색에 관해 대부분 O(1) 성능인 HashMap 사용)

일정한 수행시간과 삽입 순서를 유지하는 경우 **LinkedHashMap** 사용 (입력순서가 중요할 때)

.

## Queue (FIFO) 
> Collection을 상속받는 인터페이스

`Queue<Object> myQueue = new LinkedList<Object>();`
    
### Queue의 구현체가 ArrayList가 아니라 LinkedList인 이유
Queue는 항상 첫 번재 저장된 데이터를 삭제하므로, ArrayList와 같은 배열 기반의 자료구조를 사용하게 되면 삭제된 빈공간을 채우기 위해서 데이터의 복사가 발생하므로 매우 비효율적이다.

그래서 Queue는 ArrayList보다 데이터의 추가/삭제가 쉬운 LinkedList로 구현하는 것이 적합하다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc47wrr%2FbtqNG0s9sD1%2FGE9KaZbmsXUbPKVzOkon20%2Fimg.png" width=600px/>