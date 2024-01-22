# **Promotion(자동 타입 변환)**

Promotion은 **작은 데이터 타입이 더 큰 데이터 타입으로 자동 변환**되는 것을 말한다. 작은 데이터 타입이 큰 데이터 타입으로 캐스팅되면 데이터 손실 없이 안전하게 처리될 수 있다. 예를 들어, 정수형 데이터 타입인 short가 int로 자동으로 프로모션될 수 있다.

> **큰 크기의 타입 ← 작은 크기의 타입**
> 

즉, **byte ▶ short ▶ int ▶ long ▶ float ▶ double** 순서 또는 **char ▶ int** 순서로 자동 타입 변환이 일어난다.

```java
byte b = 10;
short s = b; // byte가 short로 Promotion됩니다.
int x = b + s; // byte와 short가 int로 Promotion되어 연산이 진행됩니다.
```

# Casting(**강제 타입 변환**)

Casting은 **변수나 표현식의 타입을 다른 타입으로 명시적으로 변환**하는 것을 의미한다. 프로그래머가 데이터 타입을 변환하고자 할 때, 명시적으로 캐스팅 연산자를 사용하여 캐스팅을 수행한다.

캐스팅은 기본 데이터 타입과 참조 데이터 타입 모두에서 발생할 수 있다.

## **기본 데이터 타입 Casting**

데이터 타입을 변환할 때는 목표 데이터 타입을 괄호 안에 써주고, 그 앞에 캐스팅 연산자인 **`(type)`**를 붙여준다.

```java
int a = (int)1.0; //컴파일 에러 해결
```

1.0이 int a 변수에 들어가면 1로 바뀌기 때문에 실제 데이터가 손실되는 것을 방지하기 위해 막고자 컴파일 에러가 나게되는데, 코드를 작성하는 프로그래머가 데이터가 손실된 다는 것을 이미 알고 있어도 이용하고 싶을 때 사용하는 것이 Casting이다.

## **참조 데이터 타입 Casting**

참조 데이터 타입 캐스팅은 객체 지향 프로그래밍에서 클래스들 사이의 상속 관계에 따라 발생한다. 업캐스팅과 다운캐스팅 두 가지 형태로 나뉜다.

### 1. **업캐스팅 (Upcasting)**

업캐스팅은 하위 클래스의 객체를 상위 클래스 타입으로 캐스팅하는 것을 말한다. 

업캐스팅은 컴파일러에 의해 자동 형변환 일어난다. 이렇게 하면 상위 클래스 타입의 변수로 하위 클래스의 객체를 다룰 수 있게 된다.

참조 가능한 영역이 축소된다.

- **서브클래스의 인스턴스의 멤버 중 공통 항목을 제외한 나머지에 대한 포기 선언을 하는 것이다.**
    
    → 대신, 하나의 슈퍼클래스 타입으로 여러 서브클래스 인스턴스를 참조할 수 있다!
    

```java
class Animal { }
class Dog extends Animal { }

Dog myDog = new Dog();
Animal myAnimal = myDog; // 업캐스팅: Dog를 Animal로 자동 캐스팅
```

### 2. **다운캐스팅 (Downcasting)**

다운캐스팅은 업캐스팅된 상위 클래스 타입의 객체를 다시 원래의 하위 클래스 타입으로 캐스팅하는 것을 말한다. 다운캐스팅은 명시적으로 프로그래머가 지정해주어야 한다. 

참조 가능한 영역이 확대된다.

- **존재하지 않는 영역에 대한 참조 위험성 때문에 명시적 형변환 후에도 오류가 발생할 수 있다**
    
    → 대부분의 다운캐스팅은 허용되지 않는다!
    

```java
Animal myAnimal = new Dog(); // 업캐스팅
Dog myDog = (Dog) myAnimal; // 다운캐스팅: Animal을 Dog로 명시적 캐스팅
```

# B**oxing, Unboxing**

Boxing은 기본 데이터 타입을 해당하는 래퍼 클래스로 변환하는 과정을 말한다. 

예를 들어, int를 Integer로 변환하는 것이 Boxing이다. Unboxing은 반대로 래퍼 클래스에서 기본 데이터 타입으로 변환하는 과정을 의미한다. Integer를 int로 변환하는 것이 Unboxing이다. 

Java와 같은 몇몇 언어에서는 자동으로 Boxing과 Unboxing이 처리되지만, 명시적으로도 수행할 수 있다.

### **Boxing**

본 데이터 타입의 값을 래퍼 클래스의 객체로 감싸는 것을 의미한다. 

예를 들어, **`int`**를 **`Integer`**로 변환하거나 **`double`**을 **`Double`**로 변환하는 것이 Boxing이다. 

Boxing은 주로 제네릭(Generic)과 같은 상황에서 기본 데이터 타입을 컬렉션에 저장하거나, 메서드 인자로 객체를 요구하는 경우에 유용하게 사용된다.

```java
int num = 42;
Integer boxedNum = num; // int를 Integer로 자동 Boxing
```

### **Unboxing**

Unboxing은 반대로 래퍼 클래스의 객체에서 기본 데이터 타입으로 변환하는 과정을 말한다. 이는 래퍼 클래스의 객체에서 실제 값만을 추출하는 것이다. 

예를 들어, **`Integer`**를 **`int`**로 변환하거나 **`Double`**을 **`double`**로 변환하는 것이 Unboxing이다. Unboxing은 주로 제네릭과 컬렉션에서 값을 꺼낼 때 사용된다.

```java
Integer boxedNum = 42;
int num = boxedNum; // Integer를 int로 자동 Unboxing
```

### ⭐️ 주의

자동으로 처리되기 때문에 때로는 예기치 않은 결과가 발생할 수 있으므로, 캐스팅과 함께 사용될 때 유의해야 한다. 특히 Null 값이 포함된 래퍼 클래스의 Unboxing은 `NullPointerException`을 유발할 수 있으므로 주의가 필요하다.

# 참고

[자바(JAVA) 18 - 레퍼런스(참조형) 형변환   업캐스팅(Up casting)과 다운캐스팅(Down Casting)](https://m.blog.naver.com/it09kim/222016609118)

[[JAVA/자바] 래퍼(Wrapper) 클래스와 박싱(boxing), 언박싱(un-boxing)](https://m.blog.naver.com/PostView.naver?blogId=heartflow89&logNo=220975218499&proxyReferer=)
