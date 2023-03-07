### AOP, Interceptor, Filter
세 가지 기능은 모두 모순 행동을 하기 전에 먼저 실행하거나, 실행한 후에 추가적인 행동을 할 때 사용되는 기능들

![image](https://user-images.githubusercontent.com/118147296/221835865-76849af1-a40d-4007-8b92-82f6b32e81be.png)

Interceptor와 Filter는 Sevlet 단위에서 실행된다. 반면 AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다. 실행 순서를 보면 Filter가 가장 밖에 있고 그 안에 Interceptor, 그 안에 AOP가 있는 형태이다.

1. 서버를 실행시켜 서블릿이 올라오는 동안에 init이 실행되고, 그 후 doFilter가 실행된다.
2. 컨트롤러에 들어가기 전 preHandler가 실행된다.
3. 컨트롤러에서 나와 prostHandler, after Completion, doFliter 순으로 된다.
4. 서블릿 종료 시 destory가 실행된다.

Filter, Intercptor, AOP 개념
### Filter (일반적으로 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리할때 사용)
말 그대로 요청과 응답을 거른 뒤 정제하는 역할을 한다.

서블릿 필터는 DispatcherSevlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청 내용을 변경하거나, 여러가지 체크를 수행할 수 있다.

또한 자원의 처리가 끝난 후 응답 내요엥 대해서도 변경하는 처리를 할 수가 있다.

보통 web.xml에 등록하고 일반적으로 잉ㄴ코딩 변환처리, xss방어 등의 요청에 대한 처리로 사용된다.

##### 필터의 실행메소드
- init() : 필터의 인스턴스 초기화
- doFilter() : 전/후 처리
- destory() : 필터 인스턴스 종료



### Interceptor (로그인 체크, 권한 체크, 프로그램 실행시간 계산작업 로그확인 등의 작업)
- 요청에 대한 작업 전/후로 가로챈다고 보면 된다.
- 필터는 스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 동작한다. 하지만 인터셉터는 스프링의 DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어들기 떄문에 스프링 컨텍스트 내부에서 Controller(Hnadler)에 관한 요청과 응답에 대해 처리한다.
- 스프링의 모든 빈 객체에 접근할 수 있다.
- 인터셉터는 여러 개를 사용할 수 있고 로그인 체크, 권한 체크, 프로그램 실행시간 계산작업 로그 확인 등의 업무처리 등을 할 수 있다. -> DS가 해당 요청을 처리 가능한 인터셉터에게 할당


따라서 아래와 같은 처리에 적합
> - 세부적인 보안 및 인증/인가 공통 작업(특정 사용자는 특정 기능을 사용 못하게 막음)
> - API 호출에 대한 로깅
> - Controller로 넘겨주는 정보(데이터)의 가공
>  (필터와 다르게 HttpServeletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없음. 대신 해당 객체가 내부적으로 갖는 값을 조작할 수 있으므로 컨트롤러로 넘겨주지 위한 정보를 가공 가능 / ex : JWT토큰 정보를 파싱해서 컨트롤러에게 사용자의 정보를 제공하도록 가공)


##### 인터셉터의 실행메소드
- preHandler() : 컨트롤러 메소드가 실행되기 전
- postHandler() : 컨트롤러 메소드 실행 직후 view페이지 렌더링 되기 전
- afterCompletion() : view페이지가 렌더링 되고 난 후



### AOP
OOP를 보완하기 위해 나온 개념

- 객체 지향의 프로그래밍을 했을 때 중복을 줄일 수 없는 부분을 줄이기 위해 종단면(관점)에서 바라보고 처리한다.
- 주로 '로깅', '트랜잭션', '에러 처리' 등 비즈니스단의 메소드에서 조금 더 세밀하게 조정하고 싶을 때 사용한다.
- Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정이 가능하며
- Intercptor나 Filter는 주소(URL)로 대상을 구분해서 걸러내야하는 반면, AOP는 주소, 파라미터, 어노테이션 등 다양한 방법으로 대상을 지정할 수 있다. (URL기반이 아닌 PointCut 단위로 동작)
- AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이다. Advice의 경우 JoinPoint나 ProceedingJoinPoint등을 활용해서 호출한다.
- 반면 HandlerInterceptor는 Filter와 유사하게 HttpServletRequest, HttpServletResponse를 파라미터로 사용한다.

###### AOP 방식으로 Logging을 구현하는 이유
Logging 요구사항이 개인정보를 처리하는 api들에 대한 로그를 생성하는 것이었다. 

따라서 모든 api에 대한 로그와 개인정보를 처리하는 로그를 구분할 필요가 있었고, 메소드 혹은 클래스 단위의 처리가 필요해 보였으며 추후 모든 로그를 통합할 때도 유연하게 확장을 할 수 있을 것이라고 생가갛여 AOP를 구현한다.


###### AOP 사용방법
Gradle 라이브러리 적용하기
```
dependencies {
	implementation("org.springframework.boot:spring-boot-starter-aop")
    }
```

최상위 클래스에 @EnableAspectJAutoProxy 설정하기 (main이 있는 최상위 클래스에 어노테이션을 등록해 AOP(@Aspect)를 찾을 수 있게 해준다.)
```
@EnableAspectJAutoProxy
@SpringBootApplication
class LoggingApplication

fun main(args: Array<String>) {
    runApplication<LoggingApplication>(*args)
}
```

공통 기능 정의하기 (단순히 로그를 출력해 주는 기능의 코드)
```
@Aspect
@Component
class LogAspect {
     val log: Logger = LoggerFactory.getLogger(LogAspect::class.java)
    
      @Before("bean(*Controller)")
      fun beforeLog(joinPoint: JoinPoint) {
      	log.info("이름이 Controller로 끝나는 모든 @Bean 호출 전 실행되는 로그 입니다.")
      }
      
      @After("execution(* com.miot2j.service.*.*(..))
      fun beforeLog(joinPoint: JoinPoint) {
      	log.info("service 패키지내의 모든 메소드 호출 후 실행되는 로그 입니다.")
      }
}
```

###### PonintCout 설정하기
JoinPoint라는 개념을 설명하기 전에 기본적인 AOP의 개념을 설명 하겠다.
- Aspect : 위에서 설명한 흩어진 관심사를 모듈화 한 것. 주로 부가기능을 모듈화함.
- Target : Aspect를 적용하는 곳 (클래스, 메서드 .. )
- Advice : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
- JointPoint : Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능
- PointCut : JointPoint의 상세한 스펙을 정의한 것. 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음

위 코드에서 알 수 있듯이 @Before("bean(*Controller)") 와 같이 PointCut을 이름이 Controller로 끝나는 모든 @Bean 호출 전 실행 으로 설정 할 수있다.

###### AOP 포인트컷
- @Before : 대상 메소드의 수행 전
- @After : 대상 메소드의 수행 후
- @After-returning : 대상 메소드의 정상적인 수행 후
- @After-throwing : 예외 발생 후
- @Around : 대상 메소드의 수행 전/





