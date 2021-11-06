# 목차
[1. REST란?](#1.-rest란?)   
2. REST 특징   
3. REST 장단점   
4. REST하게 API를 디자인 한다는 것은 무슨 뜻일까?   
5. RESTful API 설계    
6. HTTP 응답 상세 코드   
# 1. REST란?
REST(REpresentational State Transfer)는 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.   
- 자원 : 해당 소프트웨어가 관리하는 모든 것 ex) 문서, 그림, 데이터, 해당 소프트웨어 자체   

월드와이드웹과 같은 분산 하이퍼 미디어 시스템을 위한 소프트웨어 아키텍처다.   
웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있다.   
다양한 클라이언트가 등장하고 이런 멀티 플랫폼을 지원하기 위해 빠르고 범용적인 REST의 중요도가 올라가고 있다.
   
기술적으로는 HTTP URI(Uniform Resource Interface)를 통해 자원을 명시하고 HTTP method를 통해 해당 자원에 대한 CRUD를 적용하는 것을 의미한다.   
- HTTP method : GET, POST, PUT, DELETE 등   
- URI : 웹 기술에서 사용되는 논리적 또는 물리적 리소스를 식별하는 고유한 문자열 시퀀스   
- URL : 웹 주소, 네트워크 상에서 리소스가 어디 있는지 알려주기 위한 규약으로 URI의 서브셋이다.
- URI는 식별하고, URL은 위치를 가리킨다.

# 2. REST 특징
## 1. Server - Client 구조 : 자원을 가진 쪽이 Server, 요청하는 쪽이 Client가 된다.   
## 2. Stateless : HTTP 프로토콜과 마찬가지로 REST도 stateless 특징을 가진다.    
- 세션, 쿠키 등 Client의 context를 서버에 저장하지 않으므로 구현이 단순해진다.   
- 서버는 각 요청을 완전히 별개의 것으로 인식, 서버 처리 방식에 일관성을 부여한다.   
## 3. Cachable : HTTP 프로토콜의 캐싱 기능을 가진다.   
- 캐시 사용을 통해 응답이 빨라지고 서버 트랜잭션이 발생하지 않기 때문에 성능이 향상된다.   
  - 트랜잭션 : 데이터베이스의 상태를 변화시키기 위해 수행하는 작업의 단위
## 4. Uniform Interface
REST를 다른 아키텍처 스타일과 구분짓는 가장 큰 특징은 Uniform Interface이다. 이것은 4개의 특징으로 나눠질 수 있다.   
### 4.1. Identification of Resources
REST는 자원 중심적 스타일이다. 각 자원은 URI로 고유하게 식별된다.
### 4.2. Manipulation of Resources through Representations
자원은 Representation을 통해 조작될 수 있다. 클라이언트는 서버의 자원과 직접 상호작용하지 않는다.   
이를 통해 클라이언트와 서버가 coupling 되는 것을 막을 수 있다.   
서버는 클라이언트 상관 없이 구현을 수정할 수 있고, 클라이언트는 서버를 구현하는 기술을 알 필요가 없어진다.   
### 4.3. Self Discriptive Message
각 메시지는 그 자체로 수신자가 이해할 수 있는 충분한 정보를 담아야 한다.
### 4.4. Hypermedia as the Engine of Application State   
하이퍼미디어는 하이퍼링크 되어 있는 텍스트다. 우리는 링크를 클릭해서 다른 웹 페이지 상태로 넘어간다.   
좋은 웹 사이트는 처음 URI 이후에는 알 필요가 없다. 클릭을 통해 사이트의 상태가 계속 변하기 때문이다.

## 5. Layerd System(계층화) : REST API 서버는 다중 층으로 구성될 수 있다.   
클라이언트는 통신하는 API 서버 쪽 층만 알면 되고 그 뒤에 숨겨진 층들은 몰라도 된다.   
즉, 클라이언트와 서버 사이에 프록시나 로드밸런서 등이 존재해도 클라이언트는 상관하지 않는다는 것이다.   
## 6. Code on Demand   
서버로부터 코드를 받아 클라이언트에서 실행시켜 클라이언트의 기능을 확장시킬 수 있다.   
실제로 거의 쓰이지는 않는다고 한다.

# 3. REST 장단점
- 장점
  - HTTP 프로토콜 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 갖출 필요가 없다.   
  - HTTP 프로토콜을 따르는 모든 플랫폼에서 사용이 가능하므로 멀티 플랫폼 지원 및 연동이 용이하다.   
  - 메시지가 self-descriptive 하기 때문에 의도하는 바를 쉽게 파악할 수 있다.   
  - 서버와 클라이언트의 역할을 명확하게 분리한다.   
  - 원하는 타입으로 데이터를 주고 받을 수 있다.   
- 단점
  - HTTP 메소드 형태가 제한적이다.   
  - HTTP 통신 모델에서만 지원한다.    
  - 표준이 없기 때문에 개발자마다 다르게 해석해 사용할 수 있다.
# 4. REST하게 API를 디자인 한다는 것은 무슨 뜻인가?
## 4.1 리소스와 행위를 명시적이고 직관적으로 분리한다.
- 리소스는 URI로 표현되는데 리소스가 가리키는 것은 명사로 표현되어야 한다.   
- 행위는 HTTP 메서드로 표현하고 GET(조회), POST(생성), PUT(entity 전체 수정), PATCH(entity 부분 수정), DELETE를 분명한 목적으로 사용한다.   
## 4.2 메시지는 Header와 Body를 명확하게 분리해서 사용한다.   
Header에는 서버가 행동할 판단의 근거가 되는 API 버전, 응답 받고자 하는 MIME 타입 등을 담는다.   
- MIME 타입 : 클라이언트에게 전송된 문서의 형태를 알려주기 위한 메커니즘
Body에는 entity에 대한 내용을 담는다.   
## 4.3 API 버전을 관리한다.
환경은 항상 변하기 때문에 API의 signiture가 변할 수 있다.   
특정 API를 변경할 때는 반드시 하위호환성을 보장해야 한다.
## 4.4 서버와 클라이언트가 같은 방식을 사용해서 요청하도록 한다. 
서버와 브라우저 둘 다 JSON으로 보내든 form-data 방식으로 보내든 하나로 통일한다.

# 5. RESTful API 설계 규칙
## 5.1 RESTful API 설계 시 기본 규칙
RESTful API를 설계할 때 가장 중요한 두 가지 규칙은 1. URI는 정보의 자원을 표현한다. 2. 자원에 대한 행위는 HTTP method로 표현한다.   
리소스 명은 동사보다는 명사를 사용한다. 행위의 표현이 리소스 명에 들어가면 안된다.
ex) GET /members/delete/1 (X) -> DELETE /member/1 (O)
## 5.2 RESTful API 설계 시 주의 사항
1) 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용한다. ex) http://restapi.example.com/animals/mammals/monkeys   
2) URI 마지막 문자로 슬래시(/)를 포함하지 않는다. ex) http://restapi.example.com/animals/mammals/monkeys/ (X)   
3) 불가피하게 긴 URI 경로를 표현할 때는 \_(언더바) 대신 \-(하이픈)을 사용한다.   
4) URI 경로에는 소문자만 사용한다.   
5) 파일 확장자는 URI에 포함시키지 않는다. Accept 헤더를 사용한다.    
ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)   
GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)   
6) 자원을 표현할 때 Collection과 Document로 표현한다. Collection은 복수, Document는 단수다.   
ex) http:// restapi.example.com/sports/soccer/players/13   
위 예에서 sport, players는 Collection이고 soccer, 13은 Document다.
 
# 6. HTTP 응답 상태 코드
잘 설계된 REST API는 URI만 잘 설계한 것이 아닌 그 리소스에 대한 응답을 잘 내어주는 것도 포함한다.   
정확한 응답의 상태 코드를 전달하는 것만으로도 많은 정보를 전달할 수 있기 때문이다.   
    
2xx : 클라이언트 요청이 성공적으로 수행되었다.   
3xx : 클라이언트는 요청을 완료하기 위해 추가적인 행동을 취해야 한다.   
4xx : 클라이언트의 잘못된 요청   
5xx : 서버 쪽 오류로 인한 상태 코드   
   
   더 구체적으로 보면,   
   
200 : 클라이언트의 요청을 정상적으로 수행했다.   
201 : 클라이언트가 어떤 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨(POST를 통한 리소스 생성 작업 시)   
   
301 : 클라이언트가 요청한 리소스에 대한 URI가 변경 되었을 때 (응답 시 Location Header에 변경된 URI를 적어 줘야 한다.)   
   
400 : 클라이언트의 요청이 부적절하다.   
401 : 클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했다.   
403 : 유저 인증 상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했다.   
404 : 클라이언트가 요청한 리소스를 서버가 찾을 수 없다.   
405 : 클라이언트가 요청한 리소스에서는 사용 불가능한 method를 이용했다.   
   
500 : 서버에 문제가 있을 경우 사용





