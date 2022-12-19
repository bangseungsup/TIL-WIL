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
다른 출처의 리소스 공유데 대한 허용/비허용 정책
보안이 중요하지만, 개발을 하다 보면 기능상 어쩔 수 없이 다른 출처 간의 상호작용을 해야하는 경우도 있고, 실무적으로 다른 회사 API를 이용해야 하는 상황도 존재하기 때문에 이와 같이 예외 사항을 두기 위해 CORS 정책을 허용하는 리소스 한해 다른 출처도 받아들 일 수 있게 만든다.

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
 - 위의 경우, http://localhost:3000 동일하니 다른 출처의 리소스 문제 없이 가져오게 






### SOP
SOP, Same-Origin Policy
같은 출처에서만 리소스를 공유할 수 
