### AOP, Interceptor, Filter
세 가지 기능은 모두 모순 행동르 하기 전에 먼저 실행하거나, 실행한 후에 추가적인 행동을 할 때 사용되는 기능들

![image](https://user-images.githubusercontent.com/118147296/221835865-76849af1-a40d-4007-8b92-82f6b32e81be.png)

Interceptor와 Filter는 Sevlet 단위에서 실행된다. 반명 AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다. 실행 순서를 보면 Filter가 가장 밖에 있고 그 안에 Interceptor, 그 안에 AOP가 있는 형태이다.

1. 서버를 실행시켜 서블릿이 올라오는 동안에 init이 실행되고, 그 후 doFilter가 실행된다.
2. 컨트롤러에 들어가기 전 preHandler가 실행된다.
3. 컨트롤러에서 나와 prostHandler, after Completion, doFliter 순으로 된다.
4. 서블릿 종료 시 destory가 실행된다.

Filter, Intercptor, AOP 개념
### Filter
말 그대로 요청과 응답을 거른 뒤 정제하는 역할을 한다.

서블릿 필터는 DispatcherSevlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청 내용을 변경하거나, 여러가지 체크를 수행할 수 있다.

또한 자원의 처리가 끝난 후 응답 내요엥 대해서도 변경하는 처리를 할 수가 있다.

보통 web.xml에 등록하고 일반적으로 잉ㄴ코딩 변환처리, xss방어 등의 요청에 대한 처리로 사용된다.

##### 필터의 실행메소드
- init() : 필터의 인스턴스 초기화
- doFilter() : 전/후 처리
- destory() : 필터 인스턴스 종료



### Interceptor
요청에 대한 작업 전/로로 가로챈다고 보면 된다.

필터는 스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 동작한다. 하지만 인터셉터는 스프링의 DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어들기 떄문에 스프링 컨텍스트 내부에서 Controller(Hnadler)에 관한 요청과 응답에 대해 처리한다.

스프링의 모든 빈 객체에 접근할 수 있다.

인터셉터는 여러 개를 사용할 수 있고 로그인 체크, 권한 체크, 프로그램 실행시간 계산작업 로그 확인 등의 업무처리 등을 할 수 있다.

##### 인터셉터의 실행메소드
- preHandler() : 컨트롤러 메소드가 실행되기 전
- postHandler() : 컨트롤러 메소드 실행 직후 view페이지 렌더링 되기 전
- afterCompletion() : view페이지가 렌더링 되고 난 후



### AOP
OOP를 보완하기 위해 나온 개념

객체 지향의 프로그래밍을 했을 때 중복을 줄일 수 없는 부분을 줄이기 위해 종단면(관점)에서 바라보고 처리한다.

주로 '로깅', '트랜잭션', '에러 처리' 등 비즈니스단의 메소드에서 조금 더 세밀하게 조정하고 싶을 때 사용한다.

Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정이 가능하며

Intercptor나 Filter는 주소로 대상을 구분해서 걸러내야하는 반면, AOP는 주소, 파라미터, 어노테이션 등 다양한 방법으로 대상을 지정할 수 있다.

AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이다. Advice의 경우 JoinPoint나 ProceedingJoinPoint등을 활용해서 호출한다.

반면 HandlerInterceptor는 Filter와 유사하게 HttpServletRequest, HttpServletResponse를 파라미터로 사용한다.

AOP 포인트컷
- @Before : 대상 메소드의 수행 전
- @After : 대상 메소드의 수행 후
- @After-returning : 대상 메소드의 정상적인 수행 후
- @After-throwing : 예외 발생 후
- @Around : 대상 메소드의 수행 전/
