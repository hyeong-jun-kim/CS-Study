# 가비지 컬렉션 (Garbage Collection)
- 유효하지 않은 메모리를 자동으로 제거해주는 작업이다.
- Java Application은 JVM(Java Virtual Machine) 위에서 구동되는데, JVM의 기능 중 더이상 사용하지 않는 객체를 청소하여 메모리 공간을 확보하는 작업이다.

> GC가 필요하는 이유는?
- Heap 영역에 저장되는 객체들이 계속해서 쌓이게되면 OutofMemoryException이 발생하여, 이를 방지하기 위해 주기적으로 사용하지 않는 객체들을 수집하여 제거해줘야한다.

> 예제
```
Test test = new Test();
test.setId(1L);
test.setName("seohae");

testRepository.save(test);

test = null; // 더이상 참조를 하지 않고 아래 코드에서 사용되지 않으므로 GC의 대상이 된다. 
...
```

## 가비지 컬렉터
메모리가 부족할 때 쓰레기를 정리해주는 작업(Garbage Collection)을 수행하는 프로그램을 Garbage Collector라고 부른다. GC 작업을 하는 가비지 콜렉터는 다음과 같은 일을 한다.

> 1. 메모리 할당
> 2. 사용중인 메모리 인식
> 3. 사용하지 않는 메모리 인식

## 가비지 컬렉션 용어 정리
> 힙 메모리

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/b7ac05b9-e9b6-473d-a7b7-2936abfc6418)

**1) Stop the World**

가비지 컬렉션을 수행하기 위해 JVM이 애플리케이션의 실행을 일시 정지하는 것을 말한다. 가비지 컬렉션이 실행되면, GC 작업을 맡은 스레드를 제외한 나머지 스레드는 모두 멈추게되고 GC 작업이 종료되면 재개된다.

**2) Mark**

애플리케이션이 일시 중지되면, GC는 참조되고 있는 객체와 연결된 객체를 타고 이동하며 접근 가능한 객체를 식별하는 과정이다.

**3) Sweep**

모든 객체 탐색이 끝나면, 식별(Mark)되지 않은 객체들을 메모리에서 해제시키는 과정

**4) Young**

- 새롭게 생성된 객체가 할당되는 영역이다.
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

> Minor GC
Young 영역 중 Eden 영역이 꽉 차게되면 발생한다.

**5) Old**
- young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- Old 영역은 Young 영역보다 크게 할당되며, 크기가 큰 만큼 가비지는 적게 발생한다.
- 해당 영역이 가득 차면 Major GC가 발생한다.

**6) Eden**
- 새로 생성된 객체가 할당되는 영역이다.
- Eden 영역이 꽉 차면, GC가 발생하면서 Mark(참조 여부 식별), Sweep(메모리 해제) 과정이 일어난다.
- 아직 사용중인 객체는 Survivor 영역으로 이동하며, Eden 영역은 비워진다.

**7) Survivor**
- Eden 영역이 꽉 차게되면, GC가 발생하면서 제거된 객체 외의 나머지 살아남은 객체는 다른 Survivor 영역으로 이동하게된다. (한번의 Minor GC를 경험한 객체들이 저장되는 곳)
- Survivor 두 영역 중 하나는 반드시 비어있는 상태이다. 만약, 두 영역에 모두 데이터가 존재하거나, 사용량이 0이라면 정상적인 상황이 아니다.

## GC 종류
### Minor GC

1. 자바 객체가 생성되면 처음에 Eden 영역에 저장된다.
1. Minor GC가 발생하면, Eden과 Survivor1에 살아있는 객체를 Survivor2로 복사한다.
2. 그리고 Survivor1과 Eden을 Clear한다.
3. 결과적으로 한번의 Minor GC에서 살아남은 객체만 Survivor2 영역에 남는다.
4. 그리고 다음번 Minor GC가 발생하면 Eden과 Survivor2 영역에서 살아있는 객체를 Survivor1로 복사하고 클리어한다. 결과적으로 Survivor1에만 살아있는 객체가 남게된다.
5. 이렇게 반복적으로 Survivor1, Survivor2를 왔다갔다하다가, Survivor 영역에서 오래 살아남은 객체는 Old 영역으로 옮겨진다. (이렇게 Old 영역으로 옮겨지는걸 promotion이라고 한다.)


> Survivor 영역은 왜 2개일까?

메모리의 외부 단편화 발생을 방지한다. 외부 단편화란, 메모리가 할당되고 해제되기를 반복하다보면 메모리 공간은 남지만 파편화되어있어 메모리를 할당할 수 없는 문제가 발생한다. 그래서 두개의 Survivor끼리 번갈아가며 메모리를 할당하여 이를 방지한다.


**특징**
- 속도가 매우 빠르다.
- 작은 크기의 메모리를 콜렉팅하는데 아주 효과적이다.
- Stop the world 방식이며, Stop the world 시간이 짧다.


### Major GC
- Old 영역이 가득 차면 발생한다.
- Minor GC 과정에서 삭제되지 않고, Old Generation 영역으로 옮겨진 객체 중 미사용된다고 판단하는 객체를 삭제하는 GC이다.
- Stop the World 방식이며, Stop the World 시간이 길다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/d4e06418-046b-437b-b051-7f19fd7a4711)

대표적으로 **Mark & Sweep** 알고리즘을 사용한다.
1. GC Root로부터 모든 객체들의 참조를 확인하면서 참조가 연결되지 않은 객체를 Mark한다.
2. 1)번의 작업이 끝나면, 사용되지 않는 객체를 모두 표시하고 이 표시된 객체를 Sweep한다.

>**GC Root란?**
>Runtime Data Area에서 Method Area, Native Stack, Java Stack 등에서 Heap 메모리의 Object들을 참조하는 데이터 영역
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/82ee972e-ced0-479b-97a0-70116a20a5f8)


### Full GC
- Heap 메모리 전체 영역에서 발생한다.
- Old, Young 영역 모두에서 발생하는 GC이다.
- Minor GC, Major GC 모두 실패하였거나, Young 영역과 Old 영역이 모두 가득찼을때 발생한다.
- 속도가 매우 느리다.
- Full GC가 일어나는 도중에는 순간적으로 자바 애플리케이션이 중지되기 때문에, 애플리케이션의 성능과 안정성에 큰 영향을 준다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/75b21481-263c-4b92-a74f-32704f470c70)

대표적으로 Mark & Sweep & Compact 알고리즘을 사용한다.
1. 전체 객체들의 참조를 확인하면서 참조가 되지 않은 객체를 Mark한다.
2. 1번의 작업이 끝나면, 사용되지 않는 객체를 모두 표시하고, 이 표시된 객체를 Sweep한다.
3. 메모리를 정리하여 메모리 단편화를 해결할 수 있도록 한다.

> Minor GC와 Major GC로 구분되는 이유는?

JVM은 Heap 영역을 설계할 때, 2가지 전제조건으로 설계되어있다.

1. 대부분의 객체가 금방 접근 불가능한 상태가 된다.
2. 오래된 객체에서 새로운 객체로의 참조는 드물게 발생한다.

객체는 일회성인 경우가 많고, 메모리에 오래 남아있기 때문에 young, old 영역으로 분리되어 설계되었다. 따라서 자주 발생하는 Minor GC, Old 영역이 가득 찰 때 발생하여 비교적 적게 발생하는 Minor GC로 나눠져서 발생한다.


## GC 종류
> JVM 버전에 따라 여러가지 GC 방식이 추가되고 발전된다.
> 사용자가 JVM 옵션 설정을 통해 GC를 변경할 수 있다.

### 1. Serial Garbage Collector

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/8f04a8f2-1bf5-42e8-b204-d87439aaf43b)

- 가장 단순한 방식의 GC로 싱글 스레드로 동작한다.
- 싱글 스레드로 동작하여 느리고, 그만큼 Stop the World 시간이 다른 GC에 비해 길다.
- Mark & Sweep & Compact 알고리즘을 사용한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/fc3241f6-4a75-46f2-b534-b5dd68e9f9a4)

- Compaction 후의 이미지를 보면, 객체가 존재하는 부분과 객체가 없는 부분으로 나눠져있다. 즉, Compaction은 필요없는 객체들을 지우고, 살아있는 객체를 한 곳으로 모은다.

### 2. Parallel Collector(=Throughput Collector)
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/70da5255-1d48-4448-8f61-e4f1452cfb53)

- Java 8의 default GC이다.
- Young 영역의 GC를 멀티 스레드 방식을 사용하여, Serial GC에 비해 상대적으로 Stop the World가 짧다.
- 그림에 보이듯, GC Thread가 여러개이다. GC 프로세스가 더 빠르게 동작하고, Stop the World 시간을 좀 더 줄일 수 있게 되었다.
- Mark & Sweep & Compact 알고리즘을 사용한다.


### 3. Parallel Old GC
- 위 Parallel GC에서 조금 더 업그레이드된 버전이다. Old GC도 병렬로 수행할 수 있도록 한다.
- Mark & Summary & Compaction 알고리즘을 사용한다.

> Mark & Summary & Compaction 알고리즘
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/fea0fac6-3203-414f-9725-30e58ec204b1)
> **1. Mark**: Old Generation을 Region이라는 논리적인 단위로 균일하게 나누고, 각 스레드들을 Region별로 사용되는 객체를 표시한다.
> **2. Summary**: region별 통계정보로 살아남은 객체들의 밀도가 높은 부분이 어디까지인지 dense prefix를 정한다. 오랜 기간 참조된 객체는 앞으로 사용할 확률이 높다는 가정하에 dense prefix를 기준으로 compact 영역을 줄인다.
> **3. Compaction**: destination과 source로 나누어 살아남은 객체는 destination으로 이동시키고 참조되지 않는 객체는 제거한다.

- Old GC 처리량을 늘려주기 위한 Summary 작업을 추가적으로 진행한다.


### 4. CMS Collector (Concurrent Mark-Sweep)
- Stop the World로 Java Application이 멈추는 현상을 줄이고자 만든 GC이다.
- Young 영역은 위 Parallel GC와 동일하다.
- Old 영역은 Reacable한 객체를 찾지않고 나눠서 찾는 아래 1~4단계 방식을 사용한다.

**Old 영역의 GC 과정**
1. Initial Mark : GC Root가 참조하는 객체만 마킹한다. (stop-the-world 발생하지만, 탐색 깊이가 얕아서 발생 시간이 매우 짧다.)
2. Concurrent Mark : stop-the-world  없이 진행된다. 참조하는 객체를 따라가며, 지속적으로 마킹한다. 
3. Remark : Concurrent Mark 과정에서 변경된 사항이 없는지 다시 한번 마킹하며 확정한다. (stop-the-world 발생하는데, 이 지속시간을 줄이기 위해 멀티스레드로 검증 작업을 수행한다.)
4. Concurrent Sweep : stop-the-world 없이 진행된다. 접근할 수 없는 객체를 제거한다.

위와 같이 stop-the-world의 발생 시간을 최소한으로 한다.

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/0fa3a6ba-8a6b-451b-9a3d-8ab8382e519b)
![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/c9dfde2e-629a-4902-a968-40f7ad817241)

- Compacting 하지 않는다

![image](https://github.com/hyeong-jun-kim/CS-Study/assets/53989167/c0097284-63a4-4c72-8789-216c6d23f7f5)

## 참고자료
https://github.com/NKLCWDT/cs/blob/main/Java/GarbageCollection.md