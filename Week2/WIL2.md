## Controller
- MVC 패턴의 **C**에 해당
  - Model이 데이터를 어떻게 처리할지 알려주는 역할
- 주로 사용자의 요청을 처리한 후 지정된 뷰에 모델 객체를 넘겨줌
- 역할에 따라 설계
  - A서비스에 대한 것은 A-Controller, B서비스는 B-Controller..

### 사용법
- `@Controller`
  - Spring MVC Controller
  - 주로 View를 반환하기 위해 사용
  - `@ResponseBody` 어노테이션과 같이 사용하면 RestController와 같은 기능 수행
  ```java
    @Controller
    public class Controller {
      @GetMapping("/home") //home으로 Get요청이들어오면
      public String homepage() {
        return "home.html"; //home.html생성
      }
    }
  ```
- `@RestController`
  - Spring Restful Controller
  - JSON/XML 형태의 객체 데이터 반환을 목적으로
  ```java
  @RestController // JSON으로 데이터를 주고받음을 선언합니다.
  public class ProductRestController {
  
    private final ProductService productService;
    private final ProductRepository productRepository;

    // 등록된 전체 상품 목록 조회
    @GetMapping("/api/products")
    public List<Product> getProducts() {
        return productRepository.findAll();
    }
  }
  ```


## Repository
- **저장소**
- DB에 접근하는 메서드들을 사용하기 위한 인터페이스
  - CRUD를 어떻게 할 것인지 정의해주는 계층
- Entity를 통해 데이터 테이블 생성되면 받아온 정보를 DB에 저장 & 조회
- '그러나 단순 데이터 조회만으로 처리하지 못하는 개념들이 있음'
  - 이러한 고도화된 정보 가공을 처리해주는 곳이 `Service`


## Service
1. Repository에서 얻어온 정보를 바탕으로 자바 문법을 이용하여 **가공**
2. 다시 Controller에게 정보를 보냄
- Controller는 클라이언트에, Repository는 데이터에 맞닿아서 정보를 주고받음
- 정보를 직접 CRUD하고 가공하는 과정에서 테이블 원본 정보 손실 우려
  - 정보 변동의 위험이 큰 로직은 Service에서 진행
  - 정보의 복사본인 DTO를 만들어서 로직 조작

### 전체적인 흐름
1. Client가 Request를 보냄 (`Ajax`, `Axios`, `fetch` 등..)
2. Request URL에 알맞은 Controller가 수신받음 (`@Controller`, `@RestController`)
3. Controller는 넘어온 요청을 처리하기 위해 Service 호출
4. Service는 알맞은 정보를 가공하여 Controller에게 데이터를 넘김
5. Controller는 Service의 결과물을 Client에게 전달


## TDD
: **Test**-Driven Development (테스트 주도 개발)
1. 테스트 케이스 작성 (새로운 함수를 정의하거나 초기적 결함을 점검하거나..)
2. 그 케이스를 통과하기 위한 최소한의 양의 코드 생성
3. 그 새 코드를 표준에 맞도록 리팩토링
- **테스트**가 주가 되어 개발
  - 테스트를 염두에 둔 프로그래밍 개발 방법

### 장점
- 객체 지향적인 코드 개발
  - 보다 명확한 기능과 구조 설계
  - 각각의 함수에 대한 기능 철저히 구조화시켜 코드 작성
  - 코드의 재사용성 보장
- 설계 수정 시간 단축
  - 테스트 코드만으로 설계의 구조적 문제 발견
  - `"결함은 일찍 찾을수록 고치는 비용이 적게 든다"`
- 디버깅 시간 단촉
  - 단위 테스트 기반의 테스트 코드 작성
  - 영역 분할로 인한 버그 발견 용이
- 유지 보수 용이
  - 테스트 요소들이 사용자 관점으로 정의, 진행됨
- 테스트 문서의 대체 가능
  - 정확한 테스트 근거 

## References
- <https://velog.io/@jybin96/Controller-Service-Repository-가-무엇일까>
- <https://velog.io/@seungho1216/Spring-BootController-Service-Repository에-대하여>
- <https://m.blog.naver.com/suresofttech/221569611618>
