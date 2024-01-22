# [기초] Stream
## **Collection vs Stream**

**collection**은 DVD, **stream**은 인터넷 스트리밍으로 예를 들어 설명할 수 있다.

### Collection

- 컬렉션은 **메모리에 모든 요소를 저장하는 데이터 구조**를 나타낸다.
    - 예를 들어, 배열, 리스트, 집합, 맵 등이 포함된다.
- **한 번에 모든 요소에 접근하거나 변경**할 수 있으며, 반복문을 사용하여 요소를 처리할 수 있다.
- 메모리에 모든 요소가 로드되기 때문에 큰 데이터 세트의 경우 성능 문제가 발생할 수 있다.
- **영화의 모든 프레임들이 미리 저장되어있는 DVD**

### Stream

- 스트림은 데이터 요소의 시퀀스를 나타내며, 컬렉션과 달리 요소들을 저장하지 않고 처리하는 개념이다.
- **요소가 필요할 때 스트림을 통해 요청**되고, 이는 지연 평가(lazy evaluation)를 의미한다.
    - 즉, 요청 시에만 요소가 처리된다.
- 대용량 데이터 처리에 효율적이며, 병렬 처리를 활용하여 성능을 향상시킬 수 있다.
- **사용자가 요청하는 값만 스트림에서 추출한다.**

```java

public class CollectionVsStream {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(4);
        numbers.add(5);

        // Collection 방식: for loop로 요소 접근
        System.out.println("Collection 방식:");
        for (int i = 0; i < numbers.size(); i++) {
            System.out.println(numbers.get(i));
        }

        // Stream 방식: forEach를 사용하여 요소 접근
        System.out.println("\nStream 방식:");
        numbers.stream().forEach(System.out::println);
    }
}
```

## **Stream vs Loop**

### Stream

- 스트림은 컬렉션의 요소를 처리하는 반복 작업을 추상화한 것이다.
- **간결하고 가독성이 좋으며**, 요소를 순차적으로 처리하는 방식으로 코드를 작성한다.
- **내부적으로 루프를 사용하지만, 개발자가 직접 루프를 작성하지 않고도 데이터 처리 작업을 수행**할 수 있다.

### Loop

- 루프는 **명시적으로 반복문을 사용하여 컬렉션의 요소에 접근하고 처리하는 방식**이다.
    - 예를 들어, for 루프를 사용하여 배열의 요소를 반복적으로 접근할 수 있다.
- 개발자가 요소에 직접 접근하고 조작해야 하므로 **코드가 상대적으로 더 복잡**할 수 있다.

```java
public class StreamVsLoop {
    public static void main(String[] args) {
        // Loop 방식: 1부터 5까지의 숫자 합계 구하기
        System.out.println("Loop 방식:");
        int sum1 = 0;
        for (int i = 1; i <= 5; i++) {
            sum1 += i;
        }
        System.out.println("합계: " + sum1);

        // Stream 방식: 1부터 5까지의 숫자 합계 구하기
        System.out.println("\nStream 방식:");
        int sum2 = IntStream.rangeClosed(1, 5).sum();
        System.out.println("합계: " + sum2);
    }
}
```

## **특징**

1. **파이프라인 연산 (Pipeline Operations)**
    
    스트림은 중간 연산과 최종 연산으로 구성된 파이프라인 형태로 작동한다. 중간 연산은 다른 스트림을 반환하고, 최종 연산은 결과를 반환한다.
    
2. **병렬 처리 (Parallel Processing)**
    
    스트림은 병렬 처리를 쉽게 할 수 있도록 지원한다. 대용량 데이터 처리에서 성능 향상을 가져올 수 있다.
    
3. **스트림은 데이터 소스를 변경하지 않는다**. 
    
    데이터 소스로 부터 데이터를 읽기만 할 뿐, 데이터 소스를 변경하지 않는다. 새로운 스트림을 생성하는 불변성을 가진다.
    
4. **스트림은 일회용**이다. 
    
    Iterator처럼 일회용이며, 스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다.
    
5. **스트림은 작업을 내부 반복으로 처리한다**. 
    
    내부 반복이라는 것은 반복문을 메소드의 내부에 숨겼다는 것을 의미한다. forEach()는 스트림에 정의된 메소드 중의 하나로 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다. 
    
    즉, forEach()s는 메소드 안으로 for문을 넣은 것이며 수행할 작업은 매개변수로 받는다.
    
6. **지연 평가 (Lazy Evaluation).** 
    
    요소가 필요할 때까지 스트림의 처리가 지연된다.
    
7. **Stream<Integer>와 IntStream**. 
    
    요소의 타입이 T인 스트림은 기본적으로 Stream<T>이지만, 오토박싱&언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림, IntStream, LongStream, DoubleStream이 제공된다.
    

🔍 **단점**

1. **디버깅 어려움**
    
    중간 단계의 데이터 처리가 뒤늦게 실행되므로 오류가 발생한 지점을 찾기 힘들어 디버깅이 어려울 수 있다. 
    
2. **재사용 어려움**
    
    스트림은 한 번 사용한 후에는 재사용할 수 없다. 새로운 스트림을 생성하여 원본 데이터를 다시 스트림으로 변환해야 한다.
    

## **Short circuit**

Lazy 와 더불어 스트림의 가장 큰 특징 중 하나는 `Short circuit` 이다. Short-circuiting operations 라고도 부른다.

모든 요소를 처리하지 않고도 결과를 반환할 수 있다. 

1. **filter** : 조건에 맞지 않는 요소를 걸러낸다. 많은 요소 중 일부만 필터링하여 결과를 반환할 수 있다.
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.stream.Collectors;
    
    public class FilterExample {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    
            // 짝수만 필터링하여 새로운 리스트 생성
            List<Integer> evenNumbers = numbers.stream()
                    .filter(num -> num % 2 == 0)
                    .collect(Collectors.toList());
    
            System.out.println("Even Numbers: " + evenNumbers); // 출력: Even Numbers: [2, 4, 6, 8, 10]
        }
    }
    ```
    
2. **findFirst / findAny** : 첫 번째 요소 또는 아무 요소를 찾으면 나머지 요소들은 처리하지 않고 바로 결과를 반환한다.
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.Optional;
    
    public class FindFirstFindAnyExample {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    
            // findFirst(): 스트림에서 첫 번째 요소 찾기
            Optional<Integer> firstNumber = numbers.stream()
                    .findFirst();
    
            firstNumber.ifPresent(num -> System.out.println("findFirst(): " + num)); // 출력: findFirst(): 1 (첫 번째 요소)
    
            // findAny(): 스트림에서 임의의 요소 찾기
            Optional<Integer> anyNumber = numbers.stream()
                    .findAny();
    
            anyNumber.ifPresent(num -> System.out.println("findAny(): " + num)); // 출력: findAny(): 1 또는 findAny(): 2 (임의의 요소가 출력)
        }
    }
    ```
    
3. **anyMatch** : 스트림에서 주어진 조건과 일치하는 요소가 하나라도 있는지 확인한다. 조건과 일치하는 요소를 발견하면 나머지 요소는 평가하지 않고, 바로 true를 반환한다.
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class AnyMatchExample {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    
            // 스트림의 요소 중 짝수가 있는지 확인
            boolean hasEvenNumber = numbers.stream()
                    .anyMatch(num -> num % 2 == 0);
    
            System.out.println("anyMatch(): " + hasEvenNumber); // 출력: anyMatch(): true (짝수가 있으므로 true)
        }
    }
    ```
    

이러한 short circuit 기능을 활용하면 불필요한 작업을 최소화하여 성능을 향상시킬 수 있다.

## **스트림 생성하기**

스트림을 생성하는 방법에는 여러 가지가 있다.

1. ****STREAM.OF()****
    - 가변 인자를 사용하여 스트림 생성.
    - 주어진 값들의 순서를 유지한 채로 스트림을 생성.
    
    ```java
    Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5);
    ```
    
    🧨 Stream.of() 메서드는 소스가 널이면 NullPointerException가 발생한다.
    
2. **Arrays.stream()**
    - 배열로부터 스트림 생성.
    - 기본 타입(int, long, double) 배열로도 스트림 생성 가능.
    
    ```java
    Stream<Integer> stream2 = Arrays.stream(new Integer[] {1, 2, 3, 4, 5});
    IntStream intStream = Arrays.stream(new int[] {1, 2, 3, 4, 5});
    ```
    
3. **Collection.stream()**
    - 컬렉션으로부터 스트림 생성.
    - Collection 인터페이스의 디폴트 메서드로 선언되어 있음.
    
    ```java
    List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
    Stream<Integer> stream3 = nums.stream();
    ```
    
4. **Stream.generate()**
    - 무한 스트림 생성.
    - 값을 소비하지 않고 생산만 함.
    - **`Supplier`**를 사용하여 스트림 생성.
    
    **Stream.iterate()**
    
    - 이전 값을 가지고 다음 요소를 생성.
    - 초기값과 **`UnaryOperator`**를 사용하여 스트림 생성.
    
    ```java
    Stream<Integer> infiniteStream = Stream.generate(() -> new Random().nextInt(10));
    infiniteStream.limit(5).forEach(System.out::println);
    ```
    
    ```java
    Stream<Integer> iterateStream = Stream.iterate(1, i -> i + 1);
    iterateStream.limit(5).forEach(System.out::println);
    ```
    
5. **Stream.Builder**
    - 스트림 빌더를 사용하여 스트림 생성.
    - 잘 사용되지 않는 방법이며, 일반적으로는 다른 방법을 선호.
    
    ```java
    Stream<Integer> streamFromBuilder = Stream.<Integer>builder()
            .add(1)
            .add(2)
            .add(3)
            .build();
    streamFromBuilder.forEach(System.out::println);
    ```
    

## **map vs filter**

### map

- map은 스트림의 각 요소에 대해 주어진 함수를 적용하여 새로운 값을 반환하는 중간 연산이다.
- 각 요소를 일대일로 변환한다.
    - 예를 들어, 스트림의 각 숫자를 제곱하여 새로운 스트림을 생성하는 것이 가능하다.

### filter

- filter는 주어진 조건에 맞는 요소만을 걸러내는 중간 연산이다.
- 조건을 만족하는 요소만 스트림에 남게 된다.
    - 예를 들어, 양수인 숫자만 걸러내는 것이 가능하다.

```java
public class MapAndFilterExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(-2, 5, -10, 15, 7, -3);

        // filter를 사용하여 양수만 걸러내기
        List<Integer> positiveNumbers = numbers.stream()
                .filter(num -> num > 0)
                .collect(Collectors.toList());

        // map을 사용하여 걸러낸 양수들을 제곱하여 새로운 리스트 생성
        List<Integer> squaredNumbers = positiveNumbers.stream()
                .map(num -> num * num)
                .collect(Collectors.toList());
    }
}
```

### **Loop Fusion**

map 과 filter 와 같은 중간 연산자들은 서로 다른 연산이지만, 실제로는 하나의 과정으로 병합되는데, 이러한 특징을 `Loop Fusion` 이라고 한다.

```java
public class LoopFusionExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Loop Fusion: map과 filter를 병합하여 단일 루프로 처리
        List<Integer> result = numbers.stream()
                .map(num -> num * 2) // 각 요소를 2배로 변환
                .filter(num -> num > 5) // 5보다 큰 요소만 필터링
                .toList(); // 결과를 리스트로 변환 (Java 16부터 사용 가능)

        System.out.println("Result: " + result); // 출력: Result: [6, 8, 10]
    }
}
```

# 참고

[자바 - 스트림(stream),  스트림의 특징](https://developer-yeony.tistory.com/179)

[35편. 스트림(Streams) (1)](https://blog.hexabrain.net/383)
