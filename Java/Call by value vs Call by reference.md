# Call by Value VS Call by reference


## Call by value

함수를 호출 할 때 단순히 값을 전달하는 형태의 함수 호출을 의미한다.

<br>

## Call by reference

메모리의 접근에 사용되는 주소 값을 전달하는 함수 호출을 의미한다.

<br>

## 자바에는 Call by reference가 없다?

* 모든 프로그래밍 언어의 기본 개념은 "값"과 "참조"다. 값은 스택 메모리에 저장된다.
* Java에서 원시 타입 변수는 실제 값을 저장하는 반면 다른 모든 타입 변수는 참조하는 객체의 주소를 가리키는 참조 변수를 저장한다.
* 객체의 주소를 가리키는 참조 변수가 스택 메모리에 저장된다. 참조하는 객체는 힙 메모리에 저장된다.
 
* 객체가 인수로 전달될 때마다 힙 메모리에서 원래 참조 변수와 동일한 객체 위치를 가리키는 참조 변수의 정확한 복사본이 생성된다. 
* 그 결과 메서드에서 동일한 객체를 변경할 때마다 해당 변경 사항이 원래 객체에 반영된다. 
* 그러나 전달된 참조 변수에 새 개체를 할당하면 원래 객체에 반영되지 않는다.

```java
public class CallByValue {

    @Test
    public void whenModifyingObjects_thenOriginalObjectChanged() {
        Foo a = new Foo(1);
        Foo b = new Foo(1);

        // Before Modification
        assertEquals(a.num, 1);
        assertEquals(b.num, 1);

        modify(a, b);

        // After Modification
        assertEquals(a.num, 2);
        assertEquals(b.num, 1);
    }

    public static void modify(Foo a1, Foo b1) {
        a1.num++;

        b1 = new Foo(1);
        b1.num++;
    }

    static class Foo {
        public int num;

        public Foo(int num) {
            this.num = num;
        }
    }
}
```

### 결론 

* 자바는 call by value만 존재한다.
* 자바의 reference는 reference value의 줄임말이다. reference는 다른 말로는 포인터다.
* reference 값을 call by value로 전달하면 호출된 매서드 내에서 호출 한 쪽에서 참조하고 있던 오브젝트 내부 값을 변경할 수 있다.

### 참고 자료

* https://www.baeldung.com/java-pass-by-value-or-pass-by-reference
* https://m.facebook.com/story.php?story_fbid=pfbid02ct5qtQ26ceeBcnxMw5fBHh7XMuXeiCZ1BvGA2pKekkfXvecTHWwtU5dHUS6G87vBl&id=1070166746&mibextid=9R9pXO
* https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value
