# Annotation

* 사전적 의미는 주석
* 자바에서 애노테이션이란 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종 
* 보통 @ 기호를 앞에 붙여서 사용한다. JDK 1.5 버전 이상에서 사용 가능 
* 자바 애너테이션은 클래스 파일에 임베디드되어 컴파일러에 의해 생성된 후 자바 가상머신에 포함되어 작동한다.

```
@interface 애노테이션 이름{
    타입 요소이름();  //애노테이션의 요소를 선언한다.
    ..
}
```

<br>

## 메타 애노테이션

* 메타 에노테이션은 애노테이션을 위한 애노테이션이다. 
* 즉 애노테이션에 붙이는 애노테이션으로 애노테이션을 정의할 때 에노테이션의 적용대상이나 유지기간등을 지정하는데 사용한다.

<br>

### @Retention

애노테이션이 유지되는 기간을 지정하는데 사용된다. 애노테이션의 유지 정책의 종류는 다음과 같다.

* SOURCE: 소스 파일에만 존재. 클래스파일에는 존재하지 않는다.
* CLASS: 클래스 파일에 존재. 실행시에 사용 불가능하다. 기본 값.
* RUNTIME: 클래스 파일에 존재. 실행시에 사용 가능하다.

@Override나 @SuppressWarnings처럼 컴파일러가 사용하는 애너테이션은 유지 정책이 SOURCE다. 

컴파일러를 직접 작성할 것이 아니면, 이 유지정책은 필요없다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {}
```

유지 정책을 RUNTIME으로 하면, 실행 시에 리플렉션을 통해 클래스 파일에 저장된 애노테이션의 정보를 읽어서 처리할 수 있다.

@FunctionalInterface는 @Override처럼 컴파일러가 체크해주는 애노테이션이지만,

실행 시에도 사용되므로 유지 정책이 RUNTIME으로 되어 있다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface{}
```

유지 정책 'CLASS'는 컴파일러가 애노테이션의 정보를 클래스 파일에 저장할 수 있는 있게는 하지만,

클래스 파일이 JVM에 로딩될 때는 애노테이션의 정보가 무시되어 실행 시에 애노테이션에 대한 정보를 얻을 수 없다.

이것이 'CLASS'가 유지정책의 기본값임에도 불구하고 잘 사용되지 않은 이유다.


참고로 지역 변수에 붙은 애노테이션은 컴파일러만 인식할 수 있으므로,

유지정책이 RUNTIME인 애노테이션을 지역변수에 붙여도 실행 시에는 인식되지 않는다.

<br>

### @Target

* 애너테이션이 적용가능한 대상을 지정하는데 사용된다. 
* 아래는 @SuppressWarnings를 정의한 것인데, 이 애노테이션에 적용할 수 있는 대상을 @Target으로 지정하였다. 
* 여러 개의 값을 지정할 때는 배열에서처럼 괄호{}를 사용해야한다.

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SORUCE)
public @interface SuppressWarnings{
    String[] value();
}
```

@Target으로 지정할 수 있는 애노테이션 적용대상의 종류는 아래와 같다.


* ANNOTATION_TYPE: 애너테이션
* CONSTRUCTOR: 생성자
* FIELD: 필드(멤버변수, enum상수)
* LOCAL_VARIABLE: 지역변수
* METHOD: 메소드
* PACKAGE: 패키지
* PARAMETER:	매개변수
* TYPE:	타입 (클래스, 인터페이스, enum)
* TYPE_PARAMETER: 타입 매개변수(JDK1.8)
* TYPE_USE: 타입이 사용되는 모든 곳(JDK1.8)

FIELD는 기본형에, TYPE_USE는 참조형에 사용된다는 점에 주의하자.

<br>

### @documented

* 애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 한다.
* 자바에서 제공하는 기본 애노테이션 중에 '@Override'와 '@SuppressWarnings'를 제외하고는 모두 이 메타 애너테이션이 붙어 있다.

#### javadoc이란?

* Javadoc은 Java 소스 코드에서 HTML 형식의 API 문서를 생성하기 위해 Java 언어를 위해 Sun Microsystems에서 작성한 문서 생성기
* HTML 형식은 관련 문서를 함께 하이퍼 링크 할 수있는 편의성을 추가하는 데 사용된다.

<br>

## 애노테이션 프로세서

* 스프링 자바 진영 개발자들이 정말 많이 쓰는 라이브러리중에 롬복이라는 라이브러리가 존재한다. 
* 롬복은 @Getter, @Setter, @Builder 등의 애노테이션과 애노테이션 프로세서를 제공하여 표준적으로 작성해야 할 코드를 개발자 대신 생성해주는 라이브러리

> 롬복의 동작 원리는 컴파일 시점에 애노테이션 프로세서를 사용하여 소스코드의 AST(abstract syntax tree)를 조작한다.


애노테이션 프로세서는 소스코드에서 소스코드에 붙어 있는 애노테이션의 정보를 읽고 컴파일러가 컴파일하는 중에 새로운 소스코드를 생성하거나, 바이트 코드를 생성할 수도 있고, 리소스 파일을 생성할수도 있는 기능

> 즉 컴파일 단계에서 코드를 조작할 수 있는 기술