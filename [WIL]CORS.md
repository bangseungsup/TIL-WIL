## CORS
CORS, Cross-Origin Resource Sharing
교차(다른) 출처 리소스 공유

### 출처
URL은 여러가지 구성 요소로 이루어져 있다.
```
protocol +    Host  +  Path + Query String + Fragment
https://www.google.com/users/?sort=asc&page=1#foo
```
출처는 Protocol과 Host, 그리고 위에는 포함되어 있지 않지만 포트 번호까지 출처이며, 서버의 위치를 찾아가기 위해 필요한 가장 기본적인 것들이다.


#### 출처 비교와 차단은 브라우저가
- 출처를 비교하는 로직은 서버에 구현된 스펙이 아닌 브라우저에 구현된 스펙
- 서버에서는 정상적인 응답을 했지만, 브라우저에서 그렇지 못했기 때문에 생기는 에러가 CORS
- 응답 데이터는 멀쩡하지만, 브라우저 부분에서 받을 수 없기 때문에 차단
>> 다른 출처의 리소스 요청이라도, 예외를 두고 차단을 하지 않는 조항을 만들었는데, 이것이 바로 CORS 정책을 지킨 리소스 요청


#### CORS
다른 출처의 리소스 공유에 대한 허용/비허용 정책
보안이 중요하지만, 개발을 하다 보면 기능상 어쩔 수 없이 다른 출처 간의 상호작용을 해야하는 경우도 있고, 실무적으로 다른 회사 API를 이용해야 하는 상황도 존재하기 때문에 이와 같이 예외 사항을 두고 특정 리소스 한해 다른 출처도 받아들 일 수 있게 만든다.

> 브라우저의 CORS 기본 동작
>
1. 클라이언트에서 HTTP 요청의 헤더에 Origin을 담아 전달
 - 기본적으로 웹은 HTTP 프로토콜을 이용하여 서버에 요청
 - 이때 브라우저는 요청 헤더에 Origin 이라는 필드에 출처를 함께 담아 전달
```
Origin http://localhost:3000
```


2. 서버는 응답헤더에 Access-Control-Allow-Origin을 담아 클라이언트에 전달
 - 서버는 응답 헤더에 Access-Control-Allow-Origin이라는 필드를 추가하고, 이 값으로 '이 소스를 접근하는 것이 허용된 출처'를 내려 보냄
```
Access-Control-Allow-Origin http://localhost:3000
```

3. 클라이언트에서 자신이 보냈던 요청의 Origin과 서버가 보내준 Access-Control-Allow-Origin 비교
 - 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin과 서버가 보내준 응답의 Access-Control-Allow-Origin 비교한 후 차단 여부 결정
 - 만약 유효하지 않다면 그 응답을 사용하지 않고 버린다 (CORS 에러)
 - 위의 경우, http://localhost:3000 동일하니 다른 출처의 리소스 문제 없이 가져오게 됨


#### CORS 작동 방식 3가지 시나리오
##### 예비 요청
브라우저는 요청을 보낼 때 한번에 바로 보내지 않고, 먼저 예비요청을 보내 서버와 잘 통신되는지 확인한 후 본 요청을 보낸다.
- 즉, 예비 요청의 역할은 본 요청을 보내기 전에 브라우저 스스로 안전한 요청인지 미리 확인하는 것
- 이때 브라우저가 예비요청을 보내는 것을 Preflight
- 이 예비 요청의 HTTP 메소드를 GET이나 POST가 아닌 OPTIONS 라는 요청이 사용된다는 것이 특징
> 예비요청 시, Origin 헤더에는 자신이 출처(http://localhost:3000)
> Access-Contorl-Request-Method 헤더에는 실제 요청에 사용할 메소드, Access-Control-Request-Headers (ex:Content-Type) 헤더에는 실제 요청에 사용할 헤더들 설정

서버는 이 예비 요청에 대한 응답으로 어떤 것을 허용하고, 어떤 것을 금지하고 있는지에 대한 헤더 정보를 담아 브라우저로 전달
> Access-Contorl-Allow-Origin 헤더에 허용되는 Origin 목록 설정
> Access-Contorl-Request-Method 헤더에 허용되는 메소드들의 목록을 설정 (ex : POST, GET, OPTIONS)
> Access-Control-Request-Headers 헤더에 허용되는 헤더들의 목록 설정 (ex : Content-Type)
> Access-Control-Max-Age 헤더에 해당 예비 요청이 브라우저에 캐시 될 수 있는 시간을 초 단위로 설정

이후 브라우저는 보낸 예비 요청과 서버가 응답해준 정책을 비교하여 해당 요청이 안전한지 확인하고 본 요청 전송 후 서버가 본 요청에 대한 응답을 하면 최종적으로 자바스크립트에 전달


##### 단순요청
예비 요청을 생략하고 바로 서버에 직행으로 본 요청을 보낸 후, 서버가 이에 대한 응답의 헤더에 Access-Control-Allow-Origin 헤더를 보내주면 CORS 정책 위한 여부를 검사하는 방식
- 심플한 만큼 특정 조건을 만족하는 경우에만 예비 요청을 생략할 수 있음
- 요청의 메소드는 GET, POST, HEAD 중 하나여야만 한다.
- Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width 헤더일 경우 에만 적용된다.
- Content-Type 헤더가 application/x-www-form-urlencoded, multipart/form-data, text/plain중 하나여야한다. 아닐 경우 예비 요청으로 동작된다.
> 단순요청의 상황은 드물다고 할 수 있는데, 대부분 HTTP API 요청은 text/xml 이나 application/json 으로 통신하기 때문에 세번째 Content-Type이 위반되기 때문


##### 인증된 요청
인증된 요청은 클라이언트에서 서버에게 자격 인증 정보(Credential)를 실어 요청할 때 사용되는 요청
여기서 말하는 자격인증 정보란 세션 ID가 저정되어 있는 쿠키(Cookie) 혹은 Authorization 헤더에 설정하는 토큰 값 등을 일컬음

1.클라이언트에서 인증 정보를 보내도록 설정하기
기본적으로 브라우저가 제공하는 요청 API들은 별도의 옵션 없이 브라우저의 쿠키와 같은 인증과 관련된 데이터를 함부로 요청 데이터에 담지 않도록 되어 있다.
- 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credential 옵션 (3가지)
- same-origin(기본 값) : 같은 출처 간 요청에만 인증 정보를 담을 수 있음
- include : 모든 요청에 인증 정보를 담을 수 있음
- omit : 모든 요청에 인증 정보를 담지 않음
만일 이러한 별도의 설정을 해주지 않으면 쿠키 등의 인증 정보는 절대로 자동으로 서버에 전송되지 않는다.

2. 서버에서 인증도니 요청에 대한 헤더 설정하기
서버도 마찬가지로 이러한 인증도니 요청에 대해 일반적인 CORS 요처오가는 다르게 대응해줘야 한다.
- 응답 헤더의 Access-Control-Allow-Credential 항목을 true로 설정해야 함
- 응답 헤더의 Access-Control-Allow-Origin 의 값에 와이들카드 문자("*")는 사용할 수 없음
- 응답 헤더의 Access-Control-Allow-Method 의 값에 와이들카드 문자("*")는 사용할 수 없음
- 응답 헤더의 Access-Control-Allow-Headers 의 값에 와이들카드 문자("*")는 사용할 수 없음

즉, 응답의 Access-Control-Allow-Origin 헤더는 와일드카드가(*) 이 아닌 분명한 Origin 으로 설정되어야 하고, Access-Control-Allow-Credential 헤더는 true로 설정되어야 한다.

### SOP
SOP, Same-Origin Policy
같은 출처에서만 리소스를 공유할 수 있게 하는 것
