### ObjectMapper
- JSON 컨텐츠를 Java 객체로 deserializaion하거나 Java 객체를 JSON으로 serialization 할 때 사용하는 Jackson 라이브러리 클래스
- ObjectMapper는 생성비용이 비싸기 때문에 bean/static으로 처리하는게 좋음


#### 1. Dependency 추가

###### Maven의 경우, pom.xml의 <dependencies>...</dependencies>에 추가
```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.3</version>
</dependency>
```
###### Gradle의 경우, build.gradle의 dependencies{...}에 추가
```
// https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind
implementation 'com.fasterxml.jackson.core:jackson-databind:2.12.3'
```

#### 2. Basic Features
```
public class User {

    private String name;
    private int age;
    
    public User(String name, int age) {
    	this.name = name;
        this.age = age;
    }
    
    public String getName() {
    	return name;
	}
    
    public int getAge() {
    	return age;
	}
}
```

##### Java Object -> JSON
```
ObjectMapper objectMapper = new ObjectMapper();
User user = new User("Ryan", 30);

objectMapper.writeValue(new File("user.json"), user);
// 파일 출력: user.json
{"name":"Ryan","age":30}
// 문자열 출력
String userAsString = objectMapper.writeValueAsString(user);
{"name":"Ryan","age":30}
```
###### wirteValue() 메소드를 사용하여 serialization 한다.
파라미터로 JSON을 저장할 파일과 직렬화시킬 객체를 넣어주면 된다.

JSON으로 직렬화 시킬 클래스에 Getter가 존재해야 한다. Jackson 라이브러리는 Getter와 Setter를 이용하여 prefix를 잘라내고 맨 앞을 소문자로 만드는 것으로 필드를 식별한다.

그렇기 때문에 만약 직렬화 시킬 클래스에 Getter가 존재하지 않으면 클래스에서 필드를 식별하지 못하고 결국 값을 가져오지 못하여 에러가 발생한다.


##### JSON -> Java Object
```
// String to Object
String json = "{ \"name\" : \"Ryan\", \"age\" : 30 }";
User user = objectMapper.readValue(json, User.class);
// JSON File to Object
User user = objectMapper.readValue(new File("user.json"), User.class);
// JSON URL to Object
User user = objectMapper.readValue(new URL("file:user.json"), User.class);
```
###### readValue() 메소드를 사용하여 deserialization 한다.
파라미터로 JSON 형태의 문자열 또는 객체와 역직렬화 시킬 클래스를 넣어주면 된다.

여기서 주의할 점은 역직렬화 시킬 클래스에 JSON을 파싱한 결과를 전달할 생성자가 있어야 한다.

기본 생성자를 사용하는 방법도 있고, 생성자에 Jackson 라이브러리에 @JsonCreator 어노테이션을 쓰는 등의 방법들이 있다.
