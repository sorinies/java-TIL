## 추상 클래스(Abstract Class)
### 추상 메소드(Abstract Method)
서브클래스에서 반드시 오버라이딩해야만 사용할 수 있는 메소드를 의미.
사용 목적: _추상 메소드가 포함된 클래스_를 상속받는 서브 클래스가 반드시 추상 메소드를 구현하도록 하기 위함.
특징: 메소드의 선언부만 존재하며, 구현부는 작성되어 있지 않음.

```java 
public abstract void Dog(Animal a){}
```

### 추상 클래스(Abstract Class)
자바에서는 _하나 이상의 추상 메소드를 포함하는 클래스_를 가리켜 추상 클래스(abstract class)라고 함.
사용 목적: _반드시 사용되어야 하는 메소드_를 추상 클래스에 추상 메소드로 선언해 이 클래스를 상속받는 모든 클래스에서 이 추상 메소드를 **반드시 재정의(Override)** 하도록 하기 위함.
특징: 추상 클래스로 객체 생성은 불가능 하지만 부모 타입 참조변수로는 사용할 수 있다.


```java 
public abstract class Animal() {
	public abstract void Dog(Animal a){}
}
```

### 예제
```java
abstract class Animal { abstract void cry(); }
class Cat extends Animal { void cry() { System.out.println("냐옹냐옹!"); } }
class Dog extends Animal { void cry() { System.out.println("멍멍!"); } }

public class Polymorphism02 {
    public static void main(String[] args) {
        // Animal a = new Animal(); // 추상 클래스는 인스턴스를 생성할 수 없음.
        Cat c = new Cat();
        Dog d = new Dog();
        
        c.cry();
        d.cry();
    }
}
```

1. Animal 클래스는 추상 메소드 cry()를 가지고 있음.
2. Animal 클래스를 상속받는 서브클래스 Dog과 Cat은 cry()를 오버라이딩(필수)해 인스턴스를 생성.