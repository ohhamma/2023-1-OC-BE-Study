# 추상 클래스
- abstract class
- **추상 메서드** : `선언부`는 있는데, 구현부가 없는 메서드
  - 추상 메서드를 하나라도 갖는 클래스는 **추상 클래스**
    ```java
    public ***abstract*** class 동물 {
    	***abstract*** void 울어보세요();
    }
    ```
- 추상 클래스는 **객체를 만들 수 없는 클래스**가 된다
    - `new`를 사용할 수 **없음**
    ```java
    // 동물 짐슴 = new 짐승();
    ```
- 추상 클래스를 상속한 하위 클래스가 **추상 메서드를 오버라이딩하지 않으면 ?**
  - 컴파일 시점에 `에러` 발생
  - must implement the inherited abstract method
  - 추상 메서드의 구현 강제. 오버라이딩 강제.


# 인터페이스
- 구현 클래스 `is able to` 인터페이스
- “무엇을 할 수 있는” 표현 형태
- 기능 **구현**하도록 강제
    - 구현을 강제할 메서드 개수 적을수록 good
```java
public class 동물 {
	String myClass;

	동물() {
		myClass = "동물";
	}

	void showMe() {
		System.out.println(myClass);
	}
}

public class 포유류 extends 동물 {
	포유류() {
		myClass = "포유류";
	}
}

public interface 날수있는 {
	void fly();
}

public class 박쥐 extends 포유류 implements 날수있는 {
	박쥐() {
		myClass = "박쥐";
	}

	@Override
	public void fly() {
		System.out.println(myClass + " 날고 있삼.. 슈웅!!! 슈웅!!!");
	}
}
```
- `public` **추상 메서드**와 `public` **정적 상수**만 가질 수 있음
    ```java
    interface Speakable {
    	double PI = 3.14159;
    	final double absoluteZeroPoint = -275.15;
    
    	void sayYes();
    }
    ```
    - 메서드에 `public`, `abstract`
    - 속성에 `public`과 `static`, `final`
    ⇒ **자동**으로 자바가 알아서 붙여줌
    ```java
    interface Speakable() {
    	***public*** ***static*** ***final*** double PI = 3.14159;
    	***public*** ***static*** ***final*** double absoluteZeroPoint = -275.15;
    
    	***public*** ***abstract*** void sayYes();
    }
    ```


# static
- 클래스가 `static` 영역에 배치될 때 실행되는 코드 블록
```java
public class 동물 {
	***static {***
		System.out.println("동물 클래스 레디온!");
	***}***
}

public class Driver03 {
	public static void main(String[] args) {
		System.out.println("main 메서드 시작!");
		***동물 뽀로로 = new 동물();***
	}
}
```
- 실행 결과
  - main 메서드 시작!
  - ***동물 클래스 레디 온 !***
- 모든 패키지, 클래스가 처음으로 사용될 때 static 영역에 로딩됨
- 클래스가 `제일 처음` 사용될 때 `단 한 번` 해당 클래스의 **static 블록 실행됨**
    - 클래스의 **정적 속성**을 사용할 때
    ```java
    public class 동물 {
    	***static int age = 0;***
    
    	***static {***
    		System.out.println("동물 클래스 레디온!");
    	***}***
    }
    
    public class Driver05 {
    	public static void main(String[] args) {
    		System.out.println("main 메서드 시작!");
    		System.out.println(***Animal.age***);
    	}
    }
    ```
    - 클래스의 **정적 메서드**를 사용할 때
    - 클래스의 **인스턴스**를 **최초**로 만들 때


# final

## 클래스
```java
public ***final*** class 고양이 { }

public class 길고양이 ***extends*** 고양이 { }
```
- **상속**을 허락하지 **않음**
- cannot subclass the final class

## 변수
```java
public class 고양이 {
	***final*** static int 정적상수1 = 1;
	***final*** static int 정적상수2;

	***final*** int 객체상수1 = 1;
	***final*** int 객체상수2;

	static {
		정적상수2 = 2;

		// 상수는 한 번 초기화되면 값을 변경할 수 없음
		// 정적상수2 = 4;
	}

	고양이() {
		객체상수2 = 2;

		// 상수는 한 번 초기화되면 값을 변경할 수 없음
		// 객체상수2 = 4;

		***final*** int 지역상수1 = 1;
		***final*** int 지역상수2;

		지역상수2 = 2;
	}
}
```
- **변경** **불**가능한 상수
- 다른 언어에서 `const` 키워드 사용 (읽기 전용 상수)
    - 자바 : const **not used**
### 초기화 시점
1. `정적` 상수 : 선언시, static블록내부(정적생성자)
2. `객체` 상수 : 선언시, 객체생성자/인스턴스블록내부
3. `지역` 상수 : 선언시, 최초한번

## 메서드
```java
public class 동물 {
	***final*** void 숨쉬다() {
		System.out.println("호흡 중");
	}
}

class 포유류 extends 동물 {
	**// 에러 발생: Cannot override the final method from 동물**
	**void 숨쉬다()** {
		System.out.println("호흡 중");
	}
}
```
- **최종**
- **오버라이딩** **금지**


# 불변 객체
- immutable object
- 생성 후 그 상태를 바꿀 수 없는 객체
- 재할당은 가능하지만, 한 번 할당하면 내부 **데이터를 변경할 수 없는** 객체
- 대표적으로 `String`, `Integer`, `Boolean`
```java
class ImmutablePerson {
	private final int age;
	private final int name;

	public ImmutablePerson(int age, int name) {
		this.age = age;
		this.name = name;
	}
}
```

## 생성방법
- 불변객체의 필드가 모두 **원시 타입**일 때
    - `final` 키워드 + `setter` 구현 **X**
- **참조 타입**이 있는 경우
    - 불변 객체의 참조 변수 또한 불변이어야함
    - `객체`를 참조
        - 불변객체로 만들면됨
    - `Array`를 참조하거나
        - 생성자에서 배열을 받아 copy해서 저장
        - getter에서 clone 반환, 배열 내부값 변경 못하도록
    - `List`를 참조하거나
        - 생성시 새로운 List를 만들어 값을 복사
            - Collection의 unmodifiableList 메서드 사용, getter에서 값 추가/삭제 불가능

## 장점
- 객체에 대한 신뢰도↑
    - transaction 내에서 객체가 변하지 않기 때문에 믿고 사용
- 생성자, 접근메서드에 대한 방어 복사 필요x
- 멀티스레드 환경에서 동기화 처리없이 객체 공유

## 단점
- 객체가 가지는 값마다 새로운 객체 필요
- 메모리 누수, 성능저하 발생 가능

---
# 선택 정리 내용
💡 **책을 읽고 다음 질문에 답하시오.**

1. `**static` 으로 선언한 변수의 초기화 시점에 관한 아래 문항에 답하시오.**

    **(1) 일반적인 인스턴스 변수들은 객체 생성 시에 초기화된다. static 변수도 동일할 지에 대해 서술하시오.**
    static 변수는 초기화하는 시점이 상수의 종류에 따라 다르다.
    - `정적` 상수는 선언 시, 또는 정적 생성자인 static 블록 내부에서 초기화된다.
    - `객체` 상수는 선언 시, 또는 객체 생성자나 인스턴스 블록 내부에서 초기화된다.
    - `지역` 상수는 선언 시, 또는 최초 한 번만 초기화된다.
    
    **(2) [심화] JVM 의 동작 원리를 살펴보며, 이중에서 static 변수가 초기화되는 시점을 특정하시오.** (관련 레퍼런스를 읽어보고 답변하시면 도움이 되실 수 있습니다.)
    [[JAVA] JVM 동작원리 및 기본개념](<https://steady-snail.tistory.com/67>)
    - JVM의 **동작 원리**
    1. JAVA Source(.java)를 JAVA Compiler를 통해 JAVA Byte Code로 컴파일
    2. 컴파일된 바이트코드를 JVM의 Class Loader에 전달
    3. 클래스로더는 Dynamic Loading을 통해 필요한 클래스들을 로딩 및 링크하여 Runtime Data area, 즉 JVM의 메모리에 올림
    4. Execution Engine은 JVM메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행
    - 이 중에서 3번 과정, 즉 `Runtime Data Area`에 필요한 클래스들을 `로딩`하는 과정에서 static 변수가 초기화됨 (메모리 할당받음)
    - Runtime Data Area의 6가지 영역 중 `Method Area`, JVM이 시작될 때 생성되는 역엿에 static 변수가 보관됨
    
2. **불변 객체의 장단점을 정리하시오.**
- **장점**
    - 객체에 대한 신뢰도↑
        - transaction 내에서 객체가 변하지 않기 때문에 믿고 사용
    - 생성자, 접근메서드에 대한 방어 복사 필요x
    - 멀티스레드 환경에서 동기화 처리없이 객체 공유
- **단점**
    - 객체가 가지는 값마다 새로운 객체 필요
    - 메모리 누수, 성능저하 발생 가능

# References
- 김종민, 스프링 입문을 위한 자바 객체 지향의 원리와 이해, 위키북스, 2015년
- <https://velog.io/@conatuseus/Java-Immutable-Object불변객체>
