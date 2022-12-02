오늘의 공부 내용은 객체지향 작업 복기 및 CRUD 작업 진행한 것 복기하기
2주간 진행했던 작업물들을 복기하면서, 부족했던 모습 그리고 완벽하게 이해하지 못했던 내용들을 복기하는 시간을 가졌다.

다시 보니, 알고 있었다고 생각했던 것들도 새롭게 느껴지는 이 기분이 달갑지 않았지만,
보고 또 보고, 또 봤던 것을 또 보고, 뒤돌아서 한번 더 보면 언젠가는 익숙해지겠지라는 마음가짐으로 보게 될 것 같다.

### CRUD
- Create (Post)
- Read (Gst)
- Update (Put 전체 / Patch 일부)
- Delete (Delete)

#### CRUD의 새로운 개념들
##### Entity
- 데이터베이스에 쓰일 필드와 여러 엔티티간 연관관계를 정의
- 데이터 베이스는 엑셀처럼 2차원 테이블이라고 생각하면 되는데, 이 테이블에 서비스서 필요한 정보를 다 저장하고 활용
- 아래 링크 그림에서 세로의 열 부분이 Column이고, 가로의 행 부분이 Entity 객체가 된다. 그리고 이 테이블 전체가 Entity이고, 각 1개의 행들이 Entity 객체라고 생각하면 됨.
- https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb26BcK%2Fbtq0AqdUhJD%2FM3ke7l3LtH5BFDIbAtPztk%2Fimg.png

##### Controller
- 사용자의 요청이 진입하는 지점 (Entry Ponint)
- 요청에 따라 어떤 처리를 할지 결정을 Service에 넘겨줌 (그 후 Service에서 실질적으로 처리한 내용을 View에게 넘겨줌)
- 사용자 요청 -> Controller -> Service -> Contorller -> View 넘겨줌
- 대규모 서비스로 갈수록 처리해야할 서비스들이 많아지는데, 이를 하나의 클래스에서 몰아 처리할게 아니라 Controller라는 중간 제어자 역할을 만들어서 역할 분담이 가능
  (개발비용이나 유지보수비용이 대폭 줄어든다)

##### Service
- Client가 Request를 보냄 (Ajax, Axios, fetch 등)
- Request URL에 알맞는 Controller가 수신 받음 (@Controller, @RestController)
- Controller는 넘어온 요청을 처리하기 위해 Service 호출
- Service는 알맞은 정보를 가공하여 Controller에게 데이터 전달
- Controller는 Service의 결과물을 Client에게 전달
- 즉, Controller에서 넘어온 요청 및 정보를 가공하여 다시 Controller에 전달하는 역할


##### Repository
- Entity에 의해 생성된 DB에 접근하는 메소드(ex : findAll();)들을 사용하기 위한 인터페이스
- 데이터베이스에 어떤 값을 넣거나 넣어진 값을 조회하는 등의 CRUD를 해야 쓸모가 있는데, 이것을 어떻게 할 것인지 정의해주는 계층
- 보통은 JpaRepository를 상속 받도록 함으로써 기본적인 동작 가능해짐

##### Dto (Data Transfer Object)
- 계층 간 데이터 교환을 위해 사용하는 객체
- Entity 클래스 기준으로 테이블이 생성되고 스키마가 변경되는데, 화면 변경과 같은 사소한 기능 변경을 위해 테이블과 연결된 Entity 클래스를 변경하는 것이 너무나 큰 변경이 될 수 있음
- View Layer와 DB Layer 역할 분리를 하는 것이 좋은데, Controller에서는 결과값으로 여러 테이블을 조인해서 줘야 할 경우가 많기 때문에 어려움
- Controller와 Service 사이에서 강한 의존을 방지하기 위해 DTO 사용
- Service 레이어가 모듈로 분리되어 버리면 해당 Type을 쓸 수 없게 됨
- 트렌잭션ㅇ로 처리되어야 하는 DTO 항목이, 항상 요청으로 들어온 결과값과 동일하지 않을 수 있음

DTO 개념은 여전히 너무 어렵고 복잡한 내용 같다. 하지만 보고 또 보고, 계속해서 반복해서 보다가 관련된 사례를 많이 접하다 보면 언젠가는 트이지 않을까하는 기대감이 든다.
CRUD 몇 번만 더 보고 이제 통곡의 벽인 로그인 세션 공부하러 가야겠다,,
