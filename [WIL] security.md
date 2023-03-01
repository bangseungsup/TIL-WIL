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

##### 그밖의 필터
- HeaderWriterFilter : Request의 Http 해더를 검사하여 header를 추가하거나 빼주는 역할을 한다.
- CorsFilter : 허가된 사이트나 클라이언트의 요청인지 검사하는 역할을 한다.
- CsrfFilter : Post나 Put과 같이 리소스를 변경하는 요청의 경우 내가 내보냈던 리소스에서 올라온 요청인지 확인한다.
- LogoutFilter : Request가 로그아웃하겠다고 하는것인지 체크한다.
- UsernamePasswordAuthenticationFilter : username / password 로 로그인을 하려고 하는지 체크하여 승인이 되면 Authentication을 부여하고 이동 할 페이지로 이동한다.
- ConcurrentSessionFilter : 동시 접속을 허용할지 체크한다.
- BearerTokenAuthenticationFilter : Authorization 해더에 Bearer 토큰을 인증해주는 역할을 한다.
- BasicAuthenticationFilter : Authorization 해더에 Basic 토큰을 인증해주는 역할을 한다.
- RequestCacheAwareFilter : request한 내용을 다음에 필요할 수 있어서 Cache에 담아주는 역할을 한다. 다음 Request가 오면 이전의 Cache값을 줄 수 있다.
- SecurityContextHolderAwareRequestFilter : 보안 관련 Servlet 3 스펙을 지원하기 위한 필터라고 한다.
- RememberMeAuthenticationFilter : 아직 Authentication 인증이 안된 경우라면 RememberMe 쿠키를 검사해서 인증 처리해준다.
- AnonymousAuthenticationFilter : 앞선 필터를 통해 인증이 아직도 안되었으면 해당 유저는 익명 사용자라고 Authentication을 정해주는 역할을 한다. (Authentication이 Null인 것을 방지!!)
- SessionManagementFilter : 서버에서 지정한 세션정책에 맞게 사용자가 사용하고 있는지 검사하는 역할을 한다.
- ExcpetionTranslationFilter : 해당 필터 이후에 인증이나 권한 예외가 발생하면 해당 필터가 처리를 해준다.
- FilterSecurityInterceptor : 사용자가 요청한 request에 들어가고 결과를 리턴해도 되는 권한(Authorization)이 있는지를 체크한다. 해당 필터에서 권한이 없다는 결과가 나온다면 위의 - ExcpetionTranslationFilter필터에서 Exception을 처리해준다.

### Fiter Chain 확인하는 방법
WebSecurityConfigurerAdapter을 상속받아 Filter Chain을 만드는 Class위에 @EnableWebSecurity(debug = true)어노테이션을 붙여주면 현재 실행되는 Security Fiter들을 확인할 수 있다.

### Filter Chain 적용되는 URL 설정하는 방법
- 해당 filter를 동작시킬 URL을 설정하려면 http.antMatcher를 통해 설정해야한다.
- 모든 request에 대해 동작하려면 다음과 같이 http.antMatcher("/**")로 하며 api~~에 대해서 적용을 하고 싶다면 http.antMatcher("/api/**") 와 같이 해줘야한다.
- 여러 종류의 URL에 대해 여러 Filter를 만들고 싶다면 SecurityConfig 클래스를 여러개 만들어줘야하며 여러개의 sequrityConfig은 순서가 중요하여 class위에 @Order annotation을 붙여줘야한다.
- 
