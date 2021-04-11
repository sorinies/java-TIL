## 인터페이스
다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스.

### 추상 클래스와의 차이점
1. 다중 상속을 지원(class는 다중 상속이 불가능)
2. 오로지 추상 메소드와 상수 만을 포함할 수 있음 (추상 클래스는 추상 메소드, 생성자, 필드, 일반 메소드도 포함할 수 있음)

### 인터페이스의 선언
1. interface 키워드 사용.
2. 모든 필드는 public static final, 모든 메소드는 public abstract로 설정됨
3. 생략된 제어자는 컴파일 단계에서 컴파일러가 자동으로 추가 (생략 가능)

```java
public interface Electronics {
  // 인터페이스의 모든 필드는 public static final 이며, 생략시 컴파일 단계에서 자동으로 추가됨.
  public static final int KOREA_VOLT = 220;
  static final int JAPAN_VOLT = 100;
  final int USA_VOLT = 115;
  int USA_VOLT2 = 230;

  // 인터페이스의 모든 메소드는 public abstract 이며, 생략시 컴파일 단계에서 자동으로 추가됨.
  public abstract void powerOn();
  void powerOff();
}
```

### 인터페이스의 사용
#### 마커 인터페이스
아무런 기능을 수행하지 않지만, 인터페이스 간 연관성을 위한 인터페이스의 사용법

```java
public interface Samsung {
}
public interface Scanner extends Electronics, Samsung {
  public abstract void scan();
}
```