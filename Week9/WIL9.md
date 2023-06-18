# 람다

## 함수형 언어
- 비절차형 언어
- `함수` 자체가 하나의 객체가 되어 사용됨
- 동일한 입력값 → 항상 동일한 리턴값
- 외부의 영향을 받지 x
- 대표적인 언어들 : Python, JavaScript

## 람다식
: Lambda Expression
- 함수를 간단한 식으로 표현하는 방법
- 메서드의 **이름**과 **반환값** 생략 → `익명 함수`
    - 엄밀히 따지자면 `익명 객체`
    - 선언과 생성을 동시에 하는 “이름이 없는 일회용 클래스”
- 간결하면서도 간편, 이해하기 쉬움
- 식(expression)임에도 불구하고 `변수`로 사용 가능
    - 메서드의 매개변수로 전달 가능
    - 메서드 결과로 반환 가능

### 💡 **함수** vs **메서드**
- 함수 : 일반적 용어, 클래스에 독립적
- 메서드 : 객체지향개념 용어, 클래스에 종속적

## 람다식 작성
1. 메서드의 이름 & 반환타입 제거
2. 선언부와 몸통`{}` 사이에 `→` 추가
3. 끝에 `;`(세미콜론) 삭제
   
```java
int max(int a, int b) {
	return a > b ? a : b;
}

// 람다식으로 변환
(a, b) -> a > b ? a : b
```

- 만약 매개변수가 **하나**일 경우 : 선언부의 괄호`()` 생략 가능
    - 매개변수 없을 땐 생략 x
      
```java
int square(int x) {
	return x * x;
}

x -> x * x
```

- 구현부 내 문장이 **한 줄**일 때 : 중괄호`{}` 생략 가능
    - 그 한 줄이 **return**문인 경우 → 생략 불가능
      
```java
void printProductName(String prouct, int i) {
	System.out.println(prouct+" "+i) ;
}

(product, i) -> System.out.println(prouct+" "+i)
```

## 함수형 인터페이스
: Functional Interface
- 추상 메서드를 사용하려면 `함수형 인터페이스` 필요
- 단 **하나**의 추상 메서드만 선언
- `@FunctionalInterface` 애너테이션 : 두 개 이상의 추상메서드 선언 여부 확인
- 이름이 없는 익명 객체 람다식의 `이름`의 역할을 하기 위해 만들어짐
  
```java
@FunctionalInterface
interface MyFunction {
	public abstract int max(int a, int b);
}
```

```java
My function f = new MyFunction() {
							public int max(int a, int b) {
                            	return a > b ? a : b ;
                            }
                        };
int mx = f.max(10, 15) ;
```

```java
My function f = (int a, int b) -> a > b ? a : b ; // 세미콜론(;)은 참조변수 대입을 위해 필요
int mx = f.max(10, 15) ;
```

- 메서드의 **선언부**가 일치하여 대체 가능

### 매개변수 타입
- 메서드의 `매개변수` 타입으로 함수형 인터페이스 활용 가능
- 매개변수로 함수형 인터페이스 타입 객체가 들어옴 → 해당 함수 내에서 람다식 사용
  
```java
@FunctionalInterface
interface MyFunction {
	void myMethod();
}

void aMethod(MyFunction f) {
	f.myMethod();
}

Myfuction f = () -> System.out.println("람다식을 이용한 myMethod()");
aMethod(f);
```

- 참조변수 없이 표현 가능

```java
aMethod(() -> System.out.println("람다식을 이용한 myMethod()"));
```

### 반환타입
- 메서드의 `반환타입`을 함수형 인터페이스 타입으로 지정

```java
MyFunction myMethod() {
	MyFunction f = () -> {};
	return f;
}
```

- 참조변수 없이 표현 가능

```java
MyFunction myMethod() {
	return () -> {};
}
```


# 스트림
- stream
- 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것
- 데이터 소스 `추상화`, 자주 사용되는 메서드 정의
- 데이터 소스에 관계없이 같은 방식으로 다룰 수 있음
- 완전한 통일, 완전한 표준화, 재사용성↑

## 작업처리 3단계
1. Stream 만들기
2. 중간 연산, 내부 반복 작업 → 반복적으로 적용 가능
3. 최종 연산, 결과 출력 → 단 한 번만 적용 가능

```java
/*
- distinct = 중복제거
- limit = ~개 까지만 
- sorted = 정렬

- forEach = 각 요소마다 
*/
stream.distinct().limit(5).sorted().forEach(System.out::println) ;
//    |----------중간연산----------| |---------최종연산----------|
```

## 스트림 특징
- **데이터 소스를 변경하지 x** : 주로 조회/취급 조건
- **일회용** : 필요하면 다시 stream 생성
- **내부 반복 작업 처리** : `forEach`처럼 반복문을 내부에 숨긴 메서드 사용, 코드 간결
- **지연된 연산** : 최종 연산이 수행되어야 스트림 요소들이 최종 연산에서 소모됨
- **Stream<Integer>&IntStream** : 참조형만 가능, 오토 박싱
- **병렬 스트림** : 멀티쓰레드로 병렬 작업 처리 수행 easy, fork&join 프레임 웍 사용


# Optional
- 존재할 수도 있지만 안 할 수도 있는 객체
- null이 될 수도 있는 객체를 감싸고 있는 클래스
- null을 다루기 좋음

## 효과
- 명시적으로 해당 변수의 null 가능성 표현 가능
- NPE를 유발하는 null 직접 다루지 x
- null 체크 직접 하지 x

## 기본 사용법
- `Optional` 선언

```java
Optional<Order> optOrder;
```

- 객체 생성

```java
Optional<User> optUser = Optional.empty();
Optional<User> optUser = Optional.of(user);
Optional<User> optUser = Optional.ofNullable(null);
```

## 객체 존재 여부 확인
- `isPresent()` : boolean 반환
- `ifPresent()` : 객체 존재하면 괄호 안의 작업 진행

```java
Optional<String>optStr=Optional.ofNullable("test");
optStr.ifPresent(str->System.out.println(str));
```

## 담고 있는 객체 접근
- `get()` : 비어있지 않은 Optional에 사용
- `orElse`(T other) : Optional 비어있는 경우 인자 리턴
- `orElseGet`(Supplier<? extends T>other) : 비어있는 Optional 객체, 생성된 객체 반환
- `orElseThrow`(Supplier<? extends X>exceptionSupplier) : 비어있는 Optional 객체, 생성된 예외 던짐

### 💡 주로 실무에서 DB에서 데이터를 조회할 때 Optional 가장 많이 사용
- 조건에 따라 조회했을 때 무조건 리턴되는 데이터가 있지 x


# References
- <https://bombichun.tistory.com/entry/JAVA람다와-스트림Lambda-Stream>
- <https://velog.io/@yummygyudon/JAVA-람다와-스트림-Lambda-Stream>
- <https://velog.io/@yummygyudon/JAVA-람다와-스트림-Lambda-Stream-s1qzqucv>
- <https://developsd.tistory.com/127>
