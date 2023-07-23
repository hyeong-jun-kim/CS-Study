# [활용] Stream
## **필터링**

스트림에서 원하는 요소만 선택하기 위해 필터링 기능을 사용한다. 자주 사용되는 두 가지 필터링 방법은 프레디케이트(Predicate - boolean을 반환하는 함수)를 이용한 필터링과 고유 요소 필터링이다.

### **프레디케이트로 필터링**

프레디케이트를 이용하여 스트림에서 특정 조건을 만족하는 요소들만 선택할 수 있다.

```java
List<Dish< vegetarianMenu = menu.stream()
		  .filter(Dish::isVegetarian) 
		  .collect(toList());
```

### **고유 요소 필터링**

스트림에서 중복된 요소를 제거하여 고유한 요소들로 구성된 스트림을 만들 수 있다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 4, 5, 5, 6, 7, 8, 8, 9);

// 중복 제거하여 고유한 요소만 선택
List<Integer> uniqueNumbers = numbers.stream()
		   .distinct()
		   .collect(Collectors.toList()); //출력: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## **스트림 슬라이싱(자바 9)**

스트림 슬라이싱은 스트림의 요소를 조각내어 선택하는 작업을 의미한다. 

Java 9부터는 스트림의 요소를 프레디케이트(Predicate)를 이용하여 필터링하거나 처음 몇 개의 요소만 선택하거나 요소를 건너뛰는 기능이 추가되었다.

### **프레디케이트를 이용한 슬라이싱**

**`takeWhile`** 메서드를 사용하여 **스트림의 요소들 중에서 주어진 프레디케이트를 만족하는 요소들만 선택**할 수 있다. 프레디케이트가 처음으로 거짓이 되는 지점까지의 요소들이 선택된다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 조건을 만족하는 요소만 선택 (3 이하인 요소들을 선택)
List<Integer> result = numbers.stream()
				.takeWhile(number -> number <= 3)
				.collect(Collectors.toList());
```

### **스트림 축소**

**`limit`** 메서드를 사용하여 **스트림의 요소를 지정한 개수로 제한하여 축소**할 수 있다.

```java
// 1부터 10까지 숫자로 이루어진 스트림을 최대 5개의 요소로 축소
Stream.iterate(1, n -> n + 1)
      .limit(5)
      .forEach(System.out::println);
```

### **요소 건너뛰기**

**`skip`** 메서드를 사용하여 **스트림의 처음에서부터 지정한 개수만큼 요소를 건너뛴 뒤, 나머지 요소들을 선택**할 수 있다.

```java
// 1부터 10까지 숫자로 이루어진 스트림에서 처음 3개의 요소를 건너뛰고 나머지 요소들을 선택
Stream.iterate(1, n -> n + 1)
      .skip(3)
      .forEach(System.out::println);
```

## **매핑**

스트림 API의 map과 flatMap 메서드는 특정 데이터를 선택하는 기능을 제공한다. (`특정 객체에서 특정 데이터를 선택하는 작업`)

> map과 flatMap은 변환에 가까운 매핑
> 

### **스트림의 각 요소에 함수 적용하기**

**`map`**을 사용하여 **스트림의 각 요소에 함수를 적용하여 새로운 스트림을 생성**한다. 

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// 각 이름의 길이로 새로운 스트림 생성
List<Integer> nameLengths = names.stream()
        .map(String::length)
        .collect(Collectors.toList()); //출력: [5, 3, 7, 5]
```

### **flatMap**

**`flatMap`**은 **스트림의 각 요소를 다른 스트림으로 매핑하고, 이를 한 개의 스트림으로 평면화**한다. 

```java
List<List<Integer>> nestedList = Arrays.asList(
                Arrays.asList(1, 2, 3),
                Arrays.asList(4, 5, 6),
                Arrays.asList(7, 8, 9)
        );

// 중첩 리스트를 평면화하여 새로운 리스트 생성
List<Integer> flattenedList = nestedList.stream()
        .flatMap(List::stream)
        .collect(Collectors.toList()); // 출력: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## **검색과 매칭**

### **프레디케이트가 적어도 한 요소와 일치하는지 확인**

**`anyMatch`**를 사용하여 스트림의 요소 중에서 **프레디케이트(Predicate)와 일치하는 요소가 적어도 한 개 이상** 있는지 확인한다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 스트림에서 3 이상인 요소가 적어도 한 개 이상 있는지 확인
boolean hasNumberGreaterThanThree = numbers.stream()
        .anyMatch(number -> number > 3); // 출력: true
```

### **프레디케이트가 모든 요소와 일치하는지 검사**

**`allMatch`**를 사용하여 스트림의 모든 요소가 프레디케이트와 일치하는지 확인한다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 스트림의 모든 요소가 0보다 큰지 확인
boolean allNumbersGreaterThanZero = numbers.stream()
        .allMatch(number -> number > 0); // 출력: true
```

### **프레디케이트가 모든 요소와 일치하지 않는 경우를 검사**

**`noneMatch`**를 사용하여 스트림의 모든 요소가 프레디케이트와 일치하지 않는지 확인한다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 스트림의 모든 요소가 10보다 큰지 확인
boolean noneNumbersGreaterThanTen = numbers.stream()
        .noneMatch(number -> number > 10); // 출력: true
```

## **리듀싱**

### **reduce 진행 과정**

**`reduce`**는 **스트림의 모든 요소를 하나로 합치는 작업을 수행**한다. 

이때, 초기값과 이항 연산(BinaryOperator)을 사용한다.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 스트림의 모든 요소를 더하여 합산
int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b); // 출력: 15
```

### **초기값이 없는 경우**

초기값이 없는 경우에는 **`reduce`**를 호출하면 **`Optional`** 객체를 반환한다. 

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 스트림의 최댓값 구하기 (초기값 없음)
Optional<Integer> max = numbers.stream()
        .reduce(Integer::max);
max.ifPresent(System.out::println); // 출력: 5
```

## **맵 리듀스 패턴(map-reduce pattern)**

스트림의 **`map`**과 **`reduce`**를 활용하여 데이터 처리 작업을 분산하여 병렬화할 수 있다. 

맵 리듀스 패턴은 대용량 데이터 처리에 유용하며, 여러 빅 데이터 처리 프레임워크에서 사용된다.

```java
// menu라는 리스트의 요소 개수를 세는 방법
int count = menu.stream()
  .map(d -> 1) // d는 menu의 각 요소를 의미하는 임시 변수
  .reduce(0, (a,b) -> a + b); //reduce 메서드를 이용하여 스트림의 모든 요소를 합산
```

## **맵, 필터**

- 추출 : map
- 선택 : filter

## **숫자형 스트림**

```java
int calories = menu.stream()
  .map(Dish::getCalories)
  .reduce(0, Integer::sum);
```

위 과정에서는 내부적으로 합계를 계산하기 위해서 Integer가 int형으로 언박싱 된다. 따라서 박싱 비용이 소모된다.****

### **기본형 특화 스트림(primitive stream specialization)**

Java 8 이전에는 스트림은 객체 요소에 대해서만 동작하며, 기본형 데이터를 다루기 어려웠다.

 

```java
int[] numbers = {1, 2, 3, 4, 5};

        // IntStream으로 변환하여 기본형 특화 스트림 사용
        IntStream intStream = Arrays.stream(numbers);

        // 기본형 특화 스트림의 연산 수행
        int sum = intStream.sum();
        int average = intStream.average().orElse(0); // 평균 구하기 (결과가 없으면 0을 반환)
        int max = intStream.max().orElse(0); // 최댓값 구하기 (결과가 없으면 0을 반환)

        System.out.println("Sum: " + sum); // 출력: Sum: 15
        System.out.println("Average: " + average); // 출력: Average: 3
        System.out.println("Max: " + max); // 출력: Max: 5
```

Java 8부터는 기본형 특화 스트림을 이용하여 기본형 데이터를 직접 다룰 수 있게 되었다.

**숫자 스트림으로 매핑**

기본형 특화 스트림으로 매핑하여 처리할 수 있다.

```java
String[] numberStrings = {"1", "2", "3", "4", "5"};

// 문자열 배열을 IntStream으로 변환하여 기본형 특화 스트림 사용
IntStream intStream = Arrays.stream(numberStrings)
          .mapToInt(Integer::parseInt);

// 기본형 특화 스트림의 연산 수행
int sum = intStream.sum(); // 출력: Sum: 15
```

**객체 스트림으로 복원하기**

기본형 특화 스트림을 객체 스트림으로 다시 복원할 수 있다.

```java
int[] numbers = {1, 2, 3, 4, 5};

        // IntStream으로 변환하여 기본형 특화 스트림 사용
        IntStream intStream = Arrays.stream(numbers);

        // 객체 스트림으로 복원
        int[] restoredArray = intStream.boxed()
                .toArray(Integer[]::new);
System.out.println(Arrays.toString(restoredArray)); // 출력: [1, 2, 3, 4, 5]
```

**숫자 범위**

**`IntStream.range(int startInclusive, int endExclusive)`** 또는 **`IntStream.rangeClosed(int startInclusive, int endInclusive)`** 메서드를 사용하여 숫자 범위를 스트림으로 생성할 수 있다.

```java
				// 1부터 5까지의 숫자 범위 스트림 생성 (5는 포함되지 않음)
        IntStream rangeStream = IntStream.range(1, 5);
        rangeStream.forEach(System.out::print); // 출력: 1234

        // 1부터 5까지의 숫자 범위 스트림 생성 (5도 포함)
        IntStream rangeClosedStream = IntStream.rangeClosed(1, 5);
        rangeClosedStream.forEach(System.out::print); // 출력: 12345
```

# 참고
[CHAP5. 스트림 활용](https://jaeho214.tistory.com/63)

[](https://github.com/NKLCWDT/cs/blob/main/Java/스트림_활용.md)
