## MVC 패턴
: 모델-뷰-컨트롤러 (소프트웨어 디자인 패턴)
- 모델 : 데이터와 비즈니스 로직 관리
- 뷰 : 레이아웃과 화면 처리
- 컨트롤러 : 명령을 모델과 뷰 부분으로 라우팅

### 모델
: 앱이 포함해야할 *데이터* 정의
- 데이터의 상태가 변경되면 뷰에게 알림

### 뷰
: 앱의 데이터를 *보여주는* 방식 (UI)
- 표시할 데이터를 모델로부터 받음

### 컨트롤러
: 앱의 사용자로부터의 *입력에 대한 응답*
- 모델, 뷰 둘 다 업데이트
  - 사용자 입력받음(폼, 버튼) -> 모델 업데이트 -> 뷰로 전송
- 뷰만 업데이트
  - 데이터를 단순히 다른 형태로 나타내고 싶을 때
  - 항목 알파벳순 정렬 or 가격순 정렬

## API
: Application Programming Interface
- 두 소프트웨어 구성 요소가 서로 통신할 수 있게 하는 메커니즘
- 기상청의 소프트웨어 시스템에 포함된 일일 기상 데이터
  - 휴대폰의 날씨 앱이 API를 통해 이 시스템과 '대화'하여 매일 최신 날씨 정보 표시
- API 아키텍쳐
  - 클라이언트 : 요청을 보내는 애플리케이션 (모바일 앱)
  - 서버 : 응답을 보내는 애플리케이션 (기상청의 날씨 데이터베이스)
- 종류
  - SOAP API
    - 단순 객체 접근 프로토콜
    - XML을 사용하여 메시지 교환
    - 유연성 bad
  - RPC API
    - 원격 프로시저 호출
    - 서버에서 프로시저 완료 후 서버에서 출력을 클라로 전송
  - Websocket API
    - JSON 객체를 사용하여 데이터 전달
    - 클라 & 서버 간 양방향 통신 지원
    - 클라에 콜백 메시지 전송
  - REST API
    - 클라가 서버에 요청을 데이터로 전송
    - 서버가 클라 입력을 사용하여 내부 함수 시작, 출력 데이터를 클라에 반환
    - 가장 많이 사용, 유연한 API

### REST
: Representational State Transfer
- 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것
- HTTP URI를 통해 자원을 명시하고, HTTP Method를 통해 해당 자원에 대한 CRUD Operation 적용하는 것
  - URI : Uniform Resource Identifier
  - HTTP Method : GET, PUT, DELETE
    - HTTP 프로토콜의 인프라를 그대로 사용한다는 장점
  - CRUD Operation
    - Create : 생성(POST)
    - Read : 조회(GET)
    - Update : 수정(PUT)
    - Delete : 삭제(DELETE)
    - HEAD : header 정보 조회(HEAD)
- 특징
  - Server-Client
  - Stateless
    - HTTP 프로토콜이 Stateless이기 때문!!
    - 각각의 요청을 별개의 것으로 인식하고 처리
    - Client의 context를 Server에 저장하지 x
  - Cacheable
    - 대량의 요청 효율적으로 처리, 응답시간 fast
  - Layered System
    - PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체 사용
  - Code-On-Demand(optional)
  - Uniform Interface

### REST API
: REST 기반으로 서비스 API를 구현한 것
- HTTP 표준을 기반으로 구현 : 클라이언트, 서버 구현 가능
---
#### **(참고) 응답상태코드**
  - `1xx` : 전송 프로토콜 수준의 정보 교환
  - `2xx` : 클라이언트 요청이 성공적으로 수행됨
  - `3xx` : 클라이언트는 요청을 완료하기 위해 추가적인 행동을 취해야 함
  - `4xx` : 클라이언트의 잘못된 요청
  - `5xx` : 서버쪽 오류로 인한 상태코드
---
## Restful
: REST라는 아키텍처를 구현하는 웹 서비스를 나타나기 위해 사용하는 용어
- 'REST API'를 제공하는 웹 서비스 -> 'RESTful'하다

## References
- <https://developer.mozilla.org/ko/docs/Glossary/MVC>
- <https://aws.amazon.com/ko/what-is/api/>
- <https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html>
