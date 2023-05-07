# 객체지향 패러다임
- 객체지향 프로그래밍 : `OOP` (Object-Oriented Programming)
- 필요한 데이터 추상화
→ 상태와 행위를 가진 객체 생성
→ 그 객체들 간의 유기적인 상호작용을 통해 로직 구성
- 객체들의 유기적인 협력 & 결합으로 파악하고자 하는 컴퓨터 프로그래밍의 패러다임

## 장점
- 모듈 재활용 용이
- 유연하고 변경에 용이한 프로그램
- 유지보수↑ 코드 변경↓
- 코드 재사용, 코드 간결화
- 인간 친화적, 직관적인 코드

## 한계
- 함수의 비일관성 문제
    - 객체는 현재 상태 값에 의존하여 동작
    - 함수 호출할 때마다 다른 결과값
- 객체 간 의존성 증가
    - 프로그램 복잡도↑
- 객체 내 상태 변화 통제 및 추적 어려움

### 클래스
- 특정 기능을 수행하기 위해 추상화를 거쳐 `집단`에 속하는 속성과 행위를 변수와 메소드로 정의한 것
- 인스턴스와 메소드를 담아둔 컨테이너

### 객체
- 클래스 상에서 정의한 것을 실제 메모리 상에 할당된 `실체`
- 모든 실재하는 대상


# 객체지향의 4대 특성
- 캡!상추다 🥬

## 캡슐화
- Encapsulation
- 서로 연관있는 속성과 기능들을 하나의 캡슐(capsule)로 만들어 데이터를 외부로부터 `보호`하는 것
- **데이터 보호(data protection)**
    - 외부로부터 클래스에 정의된 속성과 기능들을 보호
- **데이터 은닉(data hiding)**
    - 내부의 동작을 감추고 외부에는 필요한 부분만 노출
- **접근제어자(access modifiers)**
    - 클래스나 멤버들을 외부에서 접근하지 못하도록 접근 제한
    - public, protected, default, private
- **getter/setter** 메소드
    - 모든 속성값 private으로 선언
    - getter/setter은 public으로 열려있음
    - 선택적으로 외부에 접근 허용 여부 설정 가능
- 객체 고유의 독립성과 책임 영역 안전하게 보호
- 각 객체의 자율성↑ 객체 간 결합도↓

## 상속
- ~~Inheritance~~
- 기존의 클래스를 `재활용`하여 새로운 클래스 작성
- 상위 클래스로부터 확장된 여러 하위 클래스들이 상위 클래스의 속성과 기능 사용 가능
- 각 클래스 맥락에 맞게 **메소드 오버라이딩(overriding)**으로 내용 `재정의`
    - 인터페이스 : 하위 클래스에서 구체적인 구현 강제
- 공유하는 속성과 기능 재사용, 반복적인 코드↓

## 추상화
- Abstraction
- `공통성`과 본질을 모아 추출
- 불필요한 세부사항 제거, 가장 본질저깅고 공통적인 부분만을 추출하여 표현
- 객체의 공통적인 기능과 속성을 추출하여 정의
- 추상 클래스(abstract class)와 **인터페이스(interface)**
    - 인터페이스 : 객체가 수행해야 하는 핵심적인 역할만 규정, 실제적인 구현은 각각의 객체들 내에서
- **역할과 구현의 분리**
    - 유연하고 변경에 열려이는 프로그램 설계 가능

## 다형성
- Polymorphism
- 객체지향 프로그래밍의 **꽃**
- 어떤 객체의 속성이나 기능이 상황에 따라 여러 가지 `역할`을 수행할 수 있는 성질
- 상위클래스 타입의 참조변수로 하위클래스 객체를 참조하는 것
    
    ```java
    public class Main {
    	public static void main (String[] args) {
    
    		// 원래 사용했던 객체 생성 방식
    		Car car = new Car);
    		MotorBike motorBike = new MotorBike ();
    
    		// 다형성을 활용한 객체 생성 방식
    		Vehicle car2 = new Car();
    
    		--- 이하생략 ---
    	}
    }
    ```
    
- 여러 종류의 객체를 배열로 다룰 수 있음
    - 하나의 타입만으로 여려 가지 객체 참조
    
    ```java
    public class Main {
    	public static void main(String[] args) {
    
    		// 상위 클래스 타입의 객체 배열 생성
    		Vehicle vehicles [] = new Vehicle [2];
    		vehicles [0] = new Car();
    		vehicles [1] = new MotorBike ();
    
    		for (Vehicle vehicle : vehicles) {
    			System.out.printin(vehicle.getclass()); // 각각의 클래스를 호출해주는 메서드
    		}
    	}
    }
    
    // 출력값
    class Car
    class MotorBike
    ```
    
- 역할과 구현 분리
    - 객체들 간의 직접적인 결합 피하고, 느슨한 관계 설정
        - 이 문제를 해결하기 위해 등장한 것이 **의존관계 주입(dependency injection)**
    - 유연하고 변경이 용이한 프로그램 설계
- 메소드 오버라이딩 & 메소드 오버로딩
    
    ```java
    // overloading
    class Person {
        let name: String
        var age: Int
        
        init(name: String, age: Int) {
        	self.name = name
            self.age = age 
        }
        
        func introduce() {
        	print("My name is \(name).")
        }
        
        func introduce(age: Int) {
        	print("My name is \(name) and my age is \(age).")
        }
    }
    
    // overriding 
    class student: Person {
        func introduce(age: Int) {
        	print("My name is \(name) and I'm a student.")
        }
    }
    ```
    

### overriding
- 같은 이름의 메소드가 상황에 따라 다른 역할 수행
- 인터페이스에서의 메소드 오버라이딩
- 부모 클래스가 가지고 있는 메소드를
자식 클래스가 **재정의**해서 사용

### overloading
- 같은 이름의 메소드를 여러개 가지면서
**매개 변수의 유형과 개수를 다르게** 하는 것


# 함수 호출 방법
- C언어에서는 포인터를 이용하여 매개변수의 주소값을 넘겨 참조 가능
    - Call by reference
- Java에는 **포인터가 없음**
    - 매개변수 → Call by value
    - `배열`, `클래스` → Call by reference

## Call by value
- 값에 의한 호출
- 인자로 받은 값을 `복사`하여 처리

## Call by reference
- 참조에 의한 호출
- 인자로 받은 값의 주소 참조
- `직접` 값에 영향을 줌


# References
- <https://velog.io/@dbworud/프로그래밍-패러다임-객체지향-프로그래밍OOP>
- <https://www.codestates.com/blog/content/객체-지향-프로그래밍-특징#>
- <https://sudo-minz.tistory.com/91#:~:text=Call%20by%20value(값에%20의한%20호출)는%20인자로,직접%20참조를%20하느냐%20차이다.>
