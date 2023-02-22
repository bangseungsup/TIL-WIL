### HTTP (Hyper Text Transfer Protocol)
----
HTTP란 **서버/클라이언트 모델을 따라 데이터를 주고 받기 위한 프로토콜**이다. HTTP는 인터넷에서 하이퍼텍스트를 교환하기 위한 통신 규약으로, 80번 포트를 사용하고 있다. 
따라서 HTTP 서버가 80번 포트에서 요청을 기다리고 있으며, 클라이언트는 80번 포트로 요청을 보내게 된다.

##### HTTP의 구조
HTTP는 어플리케이션 레벨의 프로토콜로 TCP/IP위에서 작동한다. HTTP는 상태를 가지고 있지 않는 Stateless 프로토콜이며 Method, Path, Version, Headers, Body 등으로 구성된다.

![image](https://user-images.githubusercontent.com/118147296/220232287-f4e19a24-4df3-4e84-9084-0bf083618b9b.png)

하지만 HTTP는 암호화되지 않은 평문 데이터를 전송하는 프로토콜이기 때문에 HTTP로 비밀번호나 주민등록 번호 등을 주고 받으면 제3자가 정보를 조회할 수 있었다. 
이 문제를 해결하기 위해 HTTPS가 등장하게 되었다.

-----------------
### HTTPS (Hyper Text Transfer Protocol Secure)
HTTPS는 대칭키 암호화 방식과 비대칭키 암호화 방식을 모두 사용하고 있다.

###### 대칭키 암호화
  - 클라이언트와 서버가 동일한 키를 사용하여 암호화/복호화 진행
  - 키가 노출되면 매우 위험하지만 연산 속도가 빠름

###### 비대칭키 암호화
  - 1개의 쌍으로 구성된 공개키와 개인키를 사용하여 암호화/복호화 진행
  - 키가 노출되어도 비교적 안전하지만 연산 속도가 느림

비대칭키 암호화는 공개키, 개인키 암호화 방식을 이용해 데이터를 암호화하고 있다. 공개 키와 개인 키는 서로를 위한 한 쌍의 키이다.
  - 공개키 : 모두에게 공개 가능한 키
  - 개인키 : 나만 가지고 알고 있어야 하는 키

> 암호화를 공개키로 하느냐 개인키로 하느냐에 따라 얻는 효과가 다른데, 공개키와 개인키로 암호화하면 각각 다음과 같은 효과
>  - 공개키 암호화 : 공개키로 암호화를 하면 공개키로만 복호화할 수 있다. -> 개인키는 나만 가지고 있으므로, 나만 볼 수 있다.
>  - 개인키 암호화 : 개인키로 암호화를 하면 공개키로만 복호화할 수 있다. -> 공개키는 모두에게 공개되어 있으므로, 내가 인증한 정보임을 알려 신뢰성을 보장할 수 있다.

![image](https://user-images.githubusercontent.com/118147296/220234452-e05444a6-e317-4548-a1f7-43a8375d7e4b.png)


#### HTTPS 동작 과정
HTTPS는 **대칭키 암호화와 비대칭키 암호화를 모두 사용**하여 빠른 연산 속도와 안정성을 모두 얻고 있다. HTTPS 연결 과정(Hand-shaking)에서는 먼저 서버와 클라이언트 간에 세션키를 교환한다.
여기서 **세션키는 주고 받는 데이터를 암호화하기 위해 사용되는 대칭키**이며, 데이터 간의 교환에는 **빠른 연산 속도가 필요하므로 세션키는 대칭키**로 만들어진다.

문제는 이 세션키를 클라이언트와 서버가 어떻게 교환할 것인냐인데, 이 과정에서 비대칭키가 사용된다. 
즉 **처음 연결을 성립하여 안전하게 세션키를 공유하는 과정에서 비대칭키가 사용**되는 것이고, 이후에 데이터를 교환하는 과정에서 빠른 연산 속도를 위해 대칭키가 사용되는 것이다.

![image](https://user-images.githubusercontent.com/118147296/220235482-9d466009-a157-4469-a23b-fd7ee68ba0cd.png)

> 실제 HTTPS 연결 과정이 성립되는 흐름을 살펴보면 다음과 같다.
> 1. 클라이언트(브라우저)가 서버로 최초 연결 시도를 함
> 2. 서버는 공개키(인증서)를 브라우저에게 넘겨줌
> 3. 브라우저는 인증서의 유효성을 검사하고 세션키를 발급함
> 4. 브라우저는 세션키를 보관하며 추가로 서버의 공개키로 세션키를 암호화하여 서버로 전송함
> 5. 서버는 개인키로 암호화된 세션키를 복호화하여 세션키를 얻음
> 6. 클라이언트와 서버는 동일한 세션키를 공유하므로 데이터를 전달할 때 세션키로 암호화/복호화를 진행함

![image](https://user-images.githubusercontent.com/118147296/220265976-411d6860-cea0-46f0-99a6-5723e199fbc6.png)


#### HTTPS의 발급 과정
위의 과정에서 추가로 살펴봐야 할 부분은 서버가 비대칭키를 발급밥ㄷ는 과정이다. 서버는 클라이언트와 세션키를 공유하기 위한 공개키를 생성해야 하는데, 일반적으로는 인증된 기관(Certificate Authority)에 공개키를 전송하여 인증서를 발급받는다.

1. A기업은 HTTP 기반의 어플리케이션에 HTTPS를 적용하기 위해 공개키/개인키를 발급
2. CA기업에게 돈을 지불하고, 공개키를 저장하는 인증서의 발급을 요청함
3. CA기업은 CA기업의 이름, 서버의 공개키, 서버의 정보 등을 기반으로 인증서를 생산하고, CA기업의 개인키로 암호화하여 A기업에게 이를 제공함
4. A기업은 클라이언트에게 암호화된 인증서를 제공함
5. 브라우저는 CA기업의 공개키를 미리 다운받아 갖고 있어, 암호화된 인증서를 복호화함
6. 암호화된 인증서를 복호화하여 얻은 A기업의 공개키로 세션키를 공유함

![image](https://user-images.githubusercontent.com/118147296/220267295-805c5db0-6237-42a8-9903-c78c95b27d8f.png)

인증서는 CA의 개인키로 암호화되었기 때문에 신뢰성을 확보할 수 있고, 클라이언트는 A기업의 공개키로 데이터를 암호화하였기 때문에 A기업만 복호화화여 원본의 데이터를 얻을 수 있다.
여기서 인증서에는 A기업의 공개키가 포함되어 있으므로 A기업의 공개키로 봐도 무방하다. 또한 브라우저에는 인증된 CA기관의 정보들이 사전에 등록되어 있어 
인증된 CA기관의 인증서가 아닐 경우에는 에러 메시지가 발생한다.

#### HTTP & HTTPS
HTTP는 암호화가 추가되지 않았기 때문에 보안이 취약한 반면, HTTPS는 안전하게 데이터를 주고받을 수 있다. 하지만 HTTPS를 이용하면 암호화/복호화 과정이 필요하기 때문에 HTTP보다 속도가 느리다.
또한 HTTPS는 인증서를 발급하고 유지하기 위한 추가 비용이 발생한다.

개인 정보와 같은 민감한 데이터를 주고 받아야 한다면 HTTPS를 이용해야 하지만, 노출이 되어도 괜찮은 단순한 정보 조회 등만을 처리하고 싶다면 HTTP를 이용하면 된다.


출처.:
https//velog.io/@bclef25/http%EC%99%80-https%EC%9D%98-%EC%B0%A8%EC%9D%B4
https://stackoverflow.com/questions/188266/how-are-ssl-certificates-verified#:~:text=Your%20web%20browser%20downloads%20the,key%20of%20the%20web%20server.&text=It%20uses%20this%20public%20key,address%20of%20the%20web%20server
https://jeong-pro.tistory.com/89
https://tiptopsecurity.com/how-does-https-work-rsa-encryption-explained/
https://mangkyu.tistory.com/98