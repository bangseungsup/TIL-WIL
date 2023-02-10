Social Login 쪽 코드를 공부하다가 궁금한 점이 생겼다.
HttpEntity와 MultiValueMap 이라는 존재였다.

```
 HttpEntity<MultiValueMap<String, String>> kakaoTokenRequest =
                new HttpEntity<>(body, headers);
        RestTemplate rt = new RestTemplate();
        ResponseEntity<String> response = rt.exchange(
                "https://kauth.kakao.com/oauth/token",
                HttpMethod.POST,
                kakaoTokenRequest,
                String.class
        );
```

##### HttpEntity / ResponseEntity
  - HttpEntity와 함께 얘기할 수 있는 것이 ResponseEntity

###### HttpEntity
공식 문서를 보면
  >public class HttpEntity
  >extends Object.
  >Represents an HTTP request or response entity, consisting of headers and body.
  >Often used in combination with the RestTemplate, like so:
  '헤더와 바디로 이루어진 Http Request, Response 엔티티를 나타낸다'
  
  ```
  @GetMapping("/handle")
  public HttpEntity<String> handle(){
    HttpHeaders responseHeaders=new HttpHeaders();
    responseHeaders.set("MyResponseHeader","MyValue");
    return new HttpEntity<>("Hello World",responseHeaders);
}
  ```
  @GetMapping 요청 시, Http Header를 설정하여 변환할 수 있다. 이 외에도 공식 문서를 보면 getHeader(), getBody 등의 메소드를 통해 Http body와 header를 반환받을 수 있고, MultiValueMap<>을 사용하여 Header와 Body를 맵핑하여 요청/응답할 수 있다.
  
  
  ###### ResponseEntity
    - 응답 자체의 독립된 값이나 표현 형태로, 사용자의 HttpRequest에 대한 응답 데이터를 포함하는 클래스
    - 따라서 HttpStatus, HttpHeaders, HttpBody를 포함
    - Spring Framework에서 제공하는 클래스인 HttpEntity<T>를 상속받으며, RestTemplate(서버와 서버간 통신을 쉽게 해줌) 및 @Controller 메소드에 사용하고 있다.
    
    
 
 ###### MultiValueMap
  스프링 공식문서
  > Extension of the Map interface that stores multiple values. ( Spring api )
  > 여러 개의 값을 저장하는 맵 인터페이스의 확장

MultiValueMap을 알기 전에 기존에 맵 형태를 보면,

1. HashMap
기존에 알고 있던 Map 형태는 Key-Value 한 쌍으로 데이터를 저장하며, **중복된 키가 존재하지 않는다.**
Map에 있는 데이터(value 값)을 뽑을 때 키를 기준으로 가져오며, 키를 리스트나 배열에 존재하는 idx(인덱스)처럼 가져와 벨류를 뽑기 때문에 시간복잡도가 0(1)이다. 

2. TreeMap
HashMap과 동일한 기능에 추가 옵션이 들어간 형태이다. TreeMap은 데이터가 들어올 때마다 Key 값에 따라 **알아서 자동으로 정렬이 된다.**

3. LinkedHashMap
HashMap과 동일한 기능에 추가 옵션이 들어간 형태이다. LinkedHashMap은 **입력순서를 보장한다.** 예를 들어, HashMap엔 입력을 C B A로 했다면, 나중에 맵에 잇는 모든 값을 출력할 떄 그대로 C B A 순으로 나오는 보장이 없지만  LinkedHashMap은 가능하다.


###### MultiValueMap
MultiValueMap은 **키의 중복이 허용된다.**

MultiValueMap을 활용하여 하나의 Key "A" 에 서로 다른 3개의 값을 저장하고 불러오게 되면, 이 값들을 전부 저장하여 불러왔을 때 리스트 형태로 반환한다.
![img1 daumcdn](https://user-images.githubusercontent.com/118147296/218049674-1b8a9517-da9d-4b8e-9823-95e960926ca4.png)

![img1 daumcdn](https://user-images.githubusercontent.com/118147296/218049694-d07aab4e-206a-4f65-b14b-26ada3fc9336.png)

일반적인 Map과 MultiValueMap을 더 자세히 비교해 보면, 기존 HashMap 형태의 경우, 중복된 값을 입력하면 가장 마지막 값이 저장된다.
```
Map<String, Integer> basicMap = new HashMap<>();
		MultiValueMap<String,Integer> multiValueMap = new LinkedMultiValueMap<>();
		basicMap.put("test",1);
		basicMap.put("test",2);
		multiValueMap.add("test",1);
		multiValueMap.add("test",2);
		System.out.println("basicMap = " + basicMap);
		System.out.println("multiValueMap = " + multiValueMap);
        
        => 실행결과
        multiValueMap = {test=[1, 2]}
		basicMap = {test=2}
```
위의 코드에선 가장 마지막으로 put된 '2'의 값이 출력된다.

반면 MultiValueMap의 경우, add를 하는 경우 key 값이 중복되어 여러 값이 입력될 경우 List 형태로 값을 나타낸다.





출처. https://velog.io/@nimoh/Spring-MultiValueMap%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C


출처. https://taehoung0102.tistory.com/182
