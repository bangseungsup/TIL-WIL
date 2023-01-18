## ResponseEntity
- httpentity를 상속 받는, 결과 데이터와 HTTP 상태 코드를 직접 제어할 수 있는 클래스
- ResponseEntity에는 사용자의 HttpResponse에 대한 응답 데이터가 포함
> Http 아키텍쳐 형태에 맞게 Response를 보내주는 것에도 의미가 있음
> 에러 코드와 같은 Http 상태 코드를 전송하고 싶은 데이터와 함께 전송할 수 있기 때문에 좀 더 세밀한 제어가 필요한 경우 사용


### ResponseEntity 구조
- HttpStatus
- HttpHeaders
- HttpBody
- 구현된 인터페이스를 보면, 바디/헤더/상태코드 순으로 생성자가 만들어짐

### HTTP header와 body의 차이점
- Http header에는 (request/response)에 대한 요구사항 포함
- Http body에는 그 내용이 포함
- Response header에는 웹서버가 웹브라우저에 응답하는 메시지가 포함
- Response body에는 해당 데이터 값이 포함

#### Response header
- 웹 브라우저가 요청한 메시지에 대하여 status, 즉 성공했는지 여부(code : 200, 403 등)와 메시지, 요청 값들이 body에 담겨있다.
> ResponseEntity 클래스를 사용하면, 결과 값 / 상태코드 / 헤더값 모두 프론트에 넘겨줄 수 있고, 에러코드 또한 섬세하게 설정해서 보내줄 수 있는 장점 존재


### ResponseEntity 사용 방법
- 이미 String 내부에서 친절하게 ResponseEntity 객체가 구현되어 있어 그래도 사용 가능
- ResponseEntity 에는 생성자가 되어 있어 status 값만 넣거나 body 만 넣어도 ResponseEntity의 나머지 값이 null로 들어감

- ResponseEntity는 내부에 정적 팩토리 메소드를 구현해 놓음
- 정적 팩토리 메소드를 사용하는게 무조건 좋진 않지만, 미리 구현되어 있어 편리함
