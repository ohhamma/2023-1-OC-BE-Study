## Dependency
: 의존성
- 객체 간 의존성, 관계
- 한 객체가 다른 객체를 사용할 때 의존성이 있다고 함


## DI
: Dependency Injection (의존성 주입)
- 외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴
- 강하게 결합된 클래스들 분리
- 유연성↑ 결합도↓
- 방법
  - 생성자 주입
  - 수정자 주입
  - 필드 주입

### 생성자 주입
: Constructor Injection
```java
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
```
- 생성자 호출 시점에 1회 호출 보장
- 주입받은 객체가 변하지 않거나, 반드시 객체의 주입이 필요한 경우 강제하기 위해 사용
  - **불변성** 보장
- `final` 키워드 사용 가능
- 생성자가 1개만 있으면 `@Autowired` 생략 가능

### 수정자 주입
: Setter Injection
```java
    private MemberService memberService;

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }
```
- 주입받는 객체가 변경될 가능성이 있는 경우 사용
  - 실제로 극히 드묾

### 필드 주입
: Field Injection
```java
    @Autowired private MemberService memberService;
```
- 외부에서 접근이 불가능 (private)
- 필드 주입은 반드시 DI 프레임워크 존재해야함
- 반드시 사용 지양하자
  - 실제 코드와 무관한 테스트코드 or 설정을 위해 사용 가능

### @Autowired
- DI를 도와주는 스프링 어노테이션
- 스프링 컨테이너에 Bean들을 모두 등록한 후에 의존성 주입
  - 이때 `@Autowired` 사용하여 필요한 인스턴스 주입


## OCP & DIP
- 인터페이스(Interface)를 사용하여 구현
- 인터페이스 예시 (`MemberRepository`)
```java
public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```
- 구현 예시 (`MemoryMemberRepository`)
```java
public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {...}

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {...}

    @Override
    public List<Member> findAll() {...}

    ...
}
```
- 구현 예시 (`JdbcMemberRepository`)
```java
public class JdbcMemberRepository implements MemberRepository {

    private final DataSource dataSource;
    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    public Member save(Member member) {...}

    @Override
    public Optional<Member> findById(Long id) {...}
    
    @Override
    public List<Member> findAll() {...}

    @Override
    public Optional<Member> findByName(String name) {...}

    ...
}
```
- 레포지토리를 Spring Bean으로 등록할 떈 단순히 `Configuration` 파일만 수정하면 됨 (너무편해)
```java
//    @Bean
//    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
//        return new JdbcMemberRepository(dataSource);
//        return new JdbcTemplateMemberRepository(dataSource);
//        return new JpaMemberRepository(em);
//    }
```

### OCP
: Open/Close Principle
- 자신의 확장에는 열려있어야 하고, 주변의 변화에는 닫혀있어야 한다

### DIP
: Dependency Inversion Principle
- 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다.
- 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.


## Spring Container
- 스프링에서 자바 객체들을 **관리**하는 공간
  - 자바 객체 : 빈(Bean)
- 빈의 생성부터 소멸까지 개발자 대신 관리해줌
- `ApplicationContext` 컨테이너
  - `BeanFactory`의 기능 포괄
- 스프링 컨테이너에 빈을 등록하여 각 객체간 의존관계 관리
  - `@Autowired`가 정상적으로 동작하려면 빈으로써 스프링 컨테이너에 들어가있어야함


## Spring Bean
- 스프링 컨테이너가 관리하는 자바 객체
  - `ApplicationContext.getBean()`과 같은 메서드로 직접 호출 가능
- 등록하는 방법
  - 컴포넌트 스캔
  - 직접 등록

### 컴포넌트 스캔
- `@Component` 어노테이션(Annotation) 사용
- 스테테오 타입(stereotype)인 `@Controller`, `@Service`, `@Repository`도 자동으로 스캔하여 등록
  - 이러한 타입들은 어노테이션 안에 `@Component` 존재
```java
@Controller
public class MemberController {...}
```

### 직접 등록
- `@Configuration` : 해당 파일에 빈을 등록할 것이니 조회해라!
- `@Bean` : 빈 직접 등록, 메서드 이름이 빈 이름으로 결정
```java
@Configuration
public class SpringConfig {
    ...

    @Bean
    public MemberService memberService() {...}

    @Bean
    public MemberRepository memberRepository() {...}
}
```

### 빈 관련 메서드
- `ApplicationContext ac;`
- `ac.메서드명` 형태로 사용

#### `getBeanDefinitionNames()`
스프링에 등록된 모든 빈 이름 조회

#### `getBean()`
특정 스프링 빈 조회
- `ac.getBean(class타입)`
- `ac.getBean(빈 이름, class타입)`
- `ac.getBean(빈 이름)`
존재하지 않으면 `NoSuchBeanDefinitionException` 예외 발생

#### `getBeansOfType()`
해당 타입에 해당하는 모든 빈 조회

> `BeanDefinition.getRole()`
> - 빈의 역할 출력
> - 크게 두 가지
>   - `ROLE_INFRASTRUCTURE` : 스프링이 내부의 동작을 위해 사용
>   - `ROLE_APPLICATION` : 사용자가 정의한 빈


![image](https://user-images.githubusercontent.com/81293930/230786963-3c661c84-f7eb-43ae-8dfe-9b2900230212.png)

## JDBC
: Java Database Connectivity
- 데이터베이스와 통신하기 위한 API
- DB의 종류에 상관없이 똑같은 코드로 해결
  - 설정한 DB에 맞는 드라이버를 사용하여 DB에 접근
  - 즉, JDBC는 **인터페이스**임

### 순수 JDBC
```java
    @Override
    public List<Member> findAll() {
        String sql = "select * from member";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        
        ...
    }
```
### JDBC Template
```java
    @Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper() {
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
```

## JPA
: Java Persistence API
- JPA가 알아서 매핑해주고 쿼리도 만들어줌
- DB와 객체를 매핑하는 기술
  - 내부적으로 DB와의 통신을 위해 JDBC 사용
- JPA도 **인터페이스**임
  - 구현체 필요 : **Hibernate** (가장 널리 사용)
```java
    @Override
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }
```

## Spring Data JPA
> Spring Data JPA, part of the larger Spring Data family, makes it easy to easily implement JPA based
> repositories. This module deals with enhanced support for JPA based data access layers. It makes it easier
> to build Spring-powered applications that use data access technologies.
- JPA를 Repository 기반으로 간편하고 효율적으로 사용할 수 있는 모듈
- Repository의 메소드를 통해 쿼리를 날릴 수 있음
  - 쿼리는 Repository의 메소드 명을 통해 생성됨
```java
    @Override
    Optional<Member> findByName(String name);
```


## References
- <https://mangkyu.tistory.com/150>
- <https://mangkyu.tistory.com/125>
- <https://m42-orion.tistory.com/100>
- <https://velog.io/@789456jang/Spring-DIP-OCP-사용-의미-이해>
- <https://mozzi-devlog.tistory.com/19>
- <https://velog.io/@tank3a/스프링-컨테이너와-스프링-빈>
- <https://velog.io/@pppp0722/JDBC-JPA-Hibernate-Spring-Data-JPA-차이>
