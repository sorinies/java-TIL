## Generic (제네릭)
데이터 타입을 일반화(Generalize)하는 것을 의미.
클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일시 미리 지정하는 방법. 이렇게 컴파일 시에 미리 타입 검사(Type Check)를 수행하면 다음과 같은 장점을 가진다.
JDK 1.5 부터 도입되었고, 그 이전에는 Object로 반환하였음.

1. 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있다.
2. 반환값에 대한 타입 변환(강제 형변환 등의 다운캐스팅 등) 및 타입 검사(instanceof 등)에 들어가는 노력을 줄일 수 있다.

### 제네릭의 선언 및 생성
```java
class MyArray<T> {
    T element;
    void setElement(T element) { this.element = element; }
    T getElement() { return element; }
}
```
위의 예제에서 사용된 'T'를 타입변수(Type Variable)라고 하며, 임의의 참조형 타입을 의미한다. (어떤 문자를 사용해도 무관하며, 여러 개의 타입 변수는 쉼표(,)로 구분하여 명시할 수 있다.)
타입 변수는 클래스에서뿐만 아니라 **메소드의 매개변수나 반환값**으로도 사용할 수 있다.

```java
MyArray<Integer> myArr = new MyArray<Integer>();
MyArray<Integer> myArr = new MyArray<>(); // Java SE 7부터 가능함 (타입 추정)
```
위처럼 제네릭 클래스를 생성할 때, 사용할 실제 타입을 명시하면 내부적으로는 정의된 타입 변수가 _명시된 실제 타입_으로 변환되어 처리된다.

#### 제네릭의 제거 시기
자바 코드에서 선언되고 사용된 제네릭 타입은 컴파일 시 컴파일러에 의해 자동으로 검사되어 타입 변환된다. 그리고 코드 내의 모든 제네릭 타입은 제거되어, 컴파일된 class 파일에는 어떠한 제네릭 타입도 포함되지 않게 된다.
이런 식으로 동작하는 이유는 제네릭을 사용하지 않는 코드와의 호환성을 유지하기 위함이다.

### 다양한 제네릭 표현
#### 타입 변수의 제한
extends 키워드를 사용하면 타입 변수에 특정 타입만을 사용하도록 제한할 수 있다.

```java
class AnimalList<T extends LandAnimal> { ... }
```

위와 같이 클래스의 타입 변수에 제한을 두면 클래스 내부에서 사용된 모든 타입 변수에 제한이 생긴다. 이때는 클래스가 아닌 인터페이스를 구현할 때에도 implements 키워드가 아닌 extends 키워드를 사용해야 한다.


---
참조
http://www.tcpschool.com/java/java_generic_concept