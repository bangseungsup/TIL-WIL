## Swagger
- Rest API를 편리하게 문서화 해주고, 이를 통해서 관리 및 제 3의 사용자가 편리하게 API를 호출해보고 테스트 할 수 있는 프로젝트
- Spirng boot에서는 간단하게 springfox-boot-starter를 gradle dependencies 추가 통해 사용 가능
- 외부에 노출되면 안되는 곳에서 사용할 땐 주의 필요

#### dependency 설정
```
implementation group: 'io.springfox', name: 'springfox-boot-starter', version: '3.0.0'
```

#### controller 설정
```
@RestController
@RequestMapping("/api")
public class SwaggerUi{

  @GetMapping("/swag")
  public String hi(){
   return "Swagger Hi"
   }
}
```

- TalendAPI를 사용하여 test하지 않고, localhost:8080/swagger-ui/ 로 접속
- 내가 만든 API 확인 및 test 가능

#### @Api Controller 설명 넣기
```
@Api (tags = "swagger 예시")
@RestController
@RequestMapping("/api")
public class SwaggerUi{

  @GetMapping("/swag")
  public String hi(){
   return "Swagger Hi"
```

- @Get 방식의 PathParam 사용과 RequsetParam 사용
```
@GetMapping("/plus/{x}")
public int plus(@PathVariable int x, @RequestParam int y){
}
```



- 추가로 @ApiParam 사용
```
@ApiParam(value = "x값")
@PathVariable int x,

@ApiParam(value = 'y값')
@RequestParam int y
```

- 이외에도 DTO, Error 부분도 따로 정할 
