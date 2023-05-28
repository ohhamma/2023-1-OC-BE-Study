# IoC
- Inversion of Control
- **제어의 역전**
- “배우들(객체)에게 필요하면 연락할테니 먼저 연락하지 마라”

## 프레임워크
- 스프링 : Controller, Service 객체의 동작을 직접 구현하지만 호출시점은 신경쓰지 x
- Junit(테스트) : 테스트 메서드 직접 작성, but 메서드 제어권은 프레임워크에
    - @Test 어노테이션을 붙여 메서드 호출
    - @BeforeEach @AfterEach
- 객체들을 프레임워크가 필요한 시점에 가져다가 프로그램 구동
- 프레임워크가 객체들 생성, 메서드 호출, 소멸 → 프로그램의 제어권이 `프레임워크`로 역전됨
    
## 라이브러리
- 다른 사람이 작성한 외부 코드
- 어플리케이션의 제어 흐름을 라이브러리에 내주지 x
    - 코드의 호출시점, 객체의 생명주기 직접 관리
- 필요한 시점에 라이브러리에 작성된 객체를 적재적소에 `가져다 씀`

## 장점
- 프로그램 진행 흐름과 구체적인 구현 분리
- 개발자는 비즈니스 로직에만 집중
- 구현제 간 변경 용이
- 객체 간 의존성↓


# 의존성 주입
- `DI` : Dependency Injection
- IoC 개념을 구현하기 위해 사용하는 디자인 패턴 중 하나
- 객체의 의존관계를 `외부`(스프링 컨테이너)에서 주입시키는 패턴

## @Autowired
- 스프링 DI에서 사용되는 어노테이션
- 변수, 메서드에 스프링이 관리하는 Bean을 자동으로 매핑
- `@Autowired` 어노테이션을 이용한 DI
    - 필드 주입
    - setter 주입
    - 생성자 주입

## 필드 주입
- Field Injection
- 클래스 2개 Bean으로 등록 (WevProject, Developer)
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class WebProject {

	@Autowired
	private Developer developer;
}
```
- WebProject에서 Developer 주입받음
- 등록된 bean을 DI컨테이너에 의해 자동으로 해당 필드에 주입

## Setter 주입
- Setter Injection
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class WebProject {

	private Developer developer;

	@Autowired
	public void setDeveloper(Developer developer) {
		this.developer = developer;
	}
}
```
- setter 메서드 만들어준 후에 @Autowired 어노테이션 붙여줌

## 생성자 주입
- Constructor Injection
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class WebProject { 
	
	private Developer developer;
	
	@Autowired
	public WebProject(Developer developer) {
		this.developer = developer;
	}
}
```
- 클래스의 필드를 파라미터로 사용하는 생성자
- 생성자 주입 사용 권고 !!

## 장점
- 의존성↓ 가독성↑ 재사용성↑
- 단위 테스트 쉬워짐


# AOP
- Aspect Oriented Programming
- `관점` 지향 프로그래밍
- 코드들을 어떤 로직의 핵심/부가적인 관점을 기준으로 부분적으로 나누어 모듈화
- Crosscutting Concerns : 흩어진 관심사
- 흩어진 관심사들을 aspect로 모듈화 → 핵심적인 비즈니스 로직에서 분리하여 재사용
- `@Aspect` 어노테이션을 붙이고 @Component를 붙여 스프링 빈으로 등록

## 특징
- 스프링 Bean에만 AOP 적용 가능
- 스프링 IoC와 연동하여 애플리케이션에서 가장 흔한 문제에 대한 해결책 지원
    - 중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관례 복잡도 증가


# References
- <https://velog.io/@ohzzi/Spring-DIIoC-IoC-DI-그게-뭔데>
- <https://codingnojam.tistory.com/9>
- <https://engkimbs.tistory.com/746>
