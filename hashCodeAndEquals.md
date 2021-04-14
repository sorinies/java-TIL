## hashCode()와 equals()
### equals()
boolean equals(Object obj)로 정의된 equals 메소드는 **2개의 객체가 동일한지 검사**하기 위해 사용된다. equals가 구현된 방법은 2개의 객체가 참조하는 것이 동일한지 확인하는 것이다.
즉, 2개의 객체가 가리키는 곳이 동일한 메모리 주소일 경우에만 동일한 객체가 된다.

```java
String s1 = new String("Test");
String s2 = new String("Test");

System.out.println(s1 == s2);			// false
System.out.println(s1.equals(s2));		// true;
```

동일한 문자열 2개를 생성하면 2개의 문자열은 서로 다른 메모리상에 할당된다. 하지만 equals()를 사용하면 true를 반환하게 되는데, 이는 String 클래스에서 equals() 메소드를 오버라이드하여 문자를 비교하는 코드를 추가했기 때문이다.

### hashCode란?
int hashCode()로 정의된 hasCode() 메소드는 실행 중(Runtime)에 객체의 유일한 integer값을 반환한다. Object 클래스에서는 heap에 저장된 객체의 메모리 주소를 반환하도록 되어 있다(예외가 존재함). 이런 특징 때문에 두 객체가 동일 객체인지 비교할 때 사용할 수 있다.

#### String.hashCode()는 값이 같으면 왜 같은 결과를 보여주는가?
String.hashCode()

```java
@Contract(pure=true) public int hashCode() {
	int h = hash;
	if (h == 0 && value.length > 0) {
		char val[] = value;
		for (int i = 0; i < value.length; i++) {
			h = 31 * h + val[i];
		}
		hash = h;
	}
	return h;
}	
```
문자열에서 한 글자씩 가져와서 정수값으로 변경하고 있다. 한 글자를 가져와서 정수와 더하면 해당 글자의 ascii 코드의 값을 사용한다.

결론적으로 주소값을 기준으로 정수값의 hashCode를 생성하는 것이 아니다. 결국 서로 다른 String 객체로 문자열이 같으면 hashCode가 같은 것이다.

hasCode는 유일한 값이라고 여겨지기 십상이지만 String에서는 중복 가능성이 있다. String의 hashCode() 메소드를 보면 31 * h + ascii 값 연산을 하기 때문에 특정 문자열 조합에서 동일한 정수값을 가지는 경우가 있다.

```java
String a = "Z@S.ME";
String b = "Z@RN.E";
```

### equals()와 hashCode()의 관계
동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다. 그렇기 때문에 만약 **equals() 메소드를 오버라이드 한다면, hashCode() 메소드도 오버라이드** 되어야 한다.



---
#### 참조
https://mangkyu.tistory.com/101
https://brunch.co.kr/@mystoryg/133