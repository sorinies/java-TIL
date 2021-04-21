## hashCode()와 equals()
equals와 hashCode는 모든 Java 객체의 부모 객체인 **Object 클래스에 정의**되어 있다. 그렇기 때문에 Java의 모든 객체는 Object 클래스에 정의된 equals와 hashCode 함수를 상속받고 있다.

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

```java
// equals, overridden in String Class 
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    } 
    return false;
}
```

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
이러한 equals와 hashCode의 관계를 정의하면 다음과 같다.

* Java 프로그램을 실행하는 동안 equals에 사용된 정보가 수정되지 않았다면, hashCode는 항상 동일한 정수값을 반환해야 한다. (Java의 프로그램을 실행할 때 마다 달라지는 것은 상관이 없다.)
* 두 객체가 equals()에 의해 동일하다면, 두 객체의 hashCode() 값도 일치해야 한다.
* 두 객체가 equals()에 의해 동일하지 않다면, 두 객체의 hashCode() 값은 일치하지 않아도 된다.

### equals()와 hashCode()의 Override
#### equals() Override의 필요성
만약 아래와 같은 Employee 클래스가 있다고 하자. Employee는 id를 고유값으로 갖는다.

```java
public class Employee{
    private Integer id;
    private String firstname;
    private String lastName;
    private String department;
 
    //Setters and Getters
}
```
만약 아래와 같이 동일한 id 값을 갖는 2개의 Employ를 서로 다른 처리 과정에 의해 얻었다고 하자. 2개의 Employee는 동일한 id를 갖기 때문에 equals 연산을 한다면 true를 반환해야 한다. 하지만 아래의 예제는 깊게 볼 필요도 없이 false를 반환할 것이다.

```java
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        System.out.println(e1.equals(e2));  //false
    }
}
```
이러한 문제를 해결하기 위해 우리는 Employ에 다음과 같은 equals 메소드를 오버라이드 해야 한다.

```java
public boolean equals(Object o) {
    if(o == null) {
        return false;
    }
    if (o == this) {
        return true;
    }
    if (getClass() != o.getClass()) {
        return false;
    }
    Employee e = (Employee) o;
    return (this.getId() == e.getId()); 
}
```
이제 equals에 의한 문제는 해결된 것 처럼 보인다. 하지만 우리가 Employee를 HashSet과 같은 자료구조에 저장하려고 하면 또 다른 문제가 생기게 된다.

#### hashCode() Override의 필요성
앞서 설명한대로 HashTable이나 HashSet, HashMap과 같은 자료구조는 _자료를 저장하기 위한 위치를 선택하기 위해 hashCode를 이용_한다. 그렇다면 수정한 Employee를 HashSet에 저장하면 어떤 결과가 나올까?

```java
import java.util.HashSet;
import java.util.Set;
 
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        System.out.println(e1.equals(e2)); // 이제 true를 반환
 
        Set<Employee> employees = new HashSet<Employee>();
        employees.add(e1);
        employees.add(e2);
         
        System.out.println(employees);  // 같은 객체로 인식하지 않았기 때문에 (중복을 허용하지 않는 Set임에도) 두개의 객체를 출력
    }
}
```
Object 클래스의 hashCode() 메소드는 해당 메모리 주소값을 반환한다고 설명하였다. 그렇기 때문에 위의 e1과 e2는 다른 해시값을 반환할 것이고, HashSet에는 2개의 객체가 서로 다른 위치에 저장될 것이다.

우리는 이러한 문제를 해결하기 위해 hashCode 메소드도 Employee 클래스에 오버라이드하여 수정해주어야 한다.


---
#### 참조
https://mangkyu.tistory.com/101
https://brunch.co.kr/@mystoryg/133