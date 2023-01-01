### 인증
- 인증이란 식별 가능한 정보 (이름 , 이메일)를 이용하여 서비스에 등록 유저의 신원 입증하는 과정

### 인가
- 인증된 회원에 한해서 기능에 대한 권한을 부여하는 것

### 세션/쿠키 방식
  1. 클라이언트가 서버로 로그인 요청을 보낸다.
  2. 서버는 클라이언트가 보낸 (id, pw)를 확인한다.
  3. (4 포함) 요청 정보가 유효하면 세션 ID를 생성한다.
  4. 클라이언트는 응답으로 받은 세션을 쿠키에 저장한다.
  5. 클라이언트가 인증이 필요한 요청을 할 때마다 헤더에 쿠키 실어서 보낸다.
  6. 서버는 쿠키를 확인하여 사용자를 식별합니다.
  7. 사용자에게 맞는 데이터를 넘겨줍니다.

### Filter Chain
- Spring Security는 FilterChainProxy라는 이름으로 내부에 여러 Filter들이 동작
- Spring Security는 Filter를 이용해서 기본적인 기능을 간단하게 구현 
- 설정은 WebSecurityConfigurerAdapter 클래스를 상속받아 오버라이딩하는 방식으로 진행
- 클라이언트가 서버에 데이터를 요청하면 DispatcherServlet에 전달되기 이전에 여러 ServletFilter를 거침
- Spring Security에 등록했었던 Filter를 이용해 사용자 보안 관련된 처리를 진행
> 이걸 FilterChain 이라 부름

### SecurityContextPersistenceFilter
- 해당 필터는 말 그대로 SecurityContext을 영속화하는 필터
- SecurityContextRepository 인터페이스를 이용하여 영속화를 진행
- 요청에 따라 repo에서(기본 설정에서는 Session 이 되겠죠?) SecurityContext를 꺼내서 SecurityContextHolder에 넣어서 요청 전반에 걸쳐 SecurityContext를 사용할 수 있게 해줌


### UsernamePasswordAuthenticationFilter
- 해당 필터는 뜻 그대로 아이디, 패스워드 기반으로 인증을 담당하는 필터

