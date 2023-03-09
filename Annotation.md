## Annotation
사전상으로는 주석의 의미, JAVA 에서는 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종 (특별한 기능을 사용 가능)

#### Annotataion 용도
- 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
- 소프트웨어 개발툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보 제공
- 실행시(런타임시)특정 기능을 실행하도록 정보를 제공

#### 자바 코드에 적용되는 내장 어노테이션
@Override
- 선언한 메서드가 오버라이드 되었다는 것을 나타냅니다.
- 만약 상위(부모) 클래스(또는 인터페이스)에서 해당 메서드를 찾을 수 없다면 컴파일 에러를 발생 시킵니다.
 
@Deprecated
- 해당 메서드가 더 이상 사용되지 않음을 표시합니다.
- 만약 사용할 경우 컴파일 경고를 발생 키십니다.
 
@SuppressWarnings
- 선언한 곳의 컴파일 경고를 무시하도록 합니다.

@SafeVarargs
- Java7 부터 지원하며, 제너릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시합니다.

@FunctionalInterface
- Java8 부터 지원하며, 함수형 인터페이스를 지정하는 어노테이션입니다.
- 만약 메서드가 존재하지 않거나, 1개 이상의 메서드(default 메서드 제외)가 존재할 경우 컴파일 오류를 발생 시킵니다.

#### 기타 어노테이션에 적용되는 어노테이션 (메타 애터네이션)
###### @Retention
자바 컴파일러가 어노테이션을 다루는 방법을 기술하며, 특정 시점까지 영향을 미치는지를 결정합니다.
- RetentionPolicy.SOURCE : 컴파일 전까지만 유효. (컴파일 이후에는 사라짐)
- RetentionPolicy.CLASS : 컴파일러가 클래스를 참조할 때까지 유효.
- RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해 계속 참조가 가능. (리플렉션 사용)

###### @Documented
해당 어노테이션을 Javadoc에 포함시킵니다.

###### @Target
어노테이션이 적용할 위치를 선택합니다.
- ElementType.PACKAGE : 패키지 선언
- ElementType.TYPE : 타입 선언
- ElementType.ANNOTATION_TYPE : 어노테이션 타입 선언
- ElementType.CONSTRUCTOR : 생성자 선언
- ElementType.FIELD : 멤버 변수 선언
- ElementType.LOCAL_VARIABLE : 지역 변수 선언
- ElementType.METHOD : 메서드 선언
- ElementType.PARAMETER : 전달인자 선언
- ElementType.TYPE_PARAMETER : 전달인자 타입 선언
- ElementType.TYPE_USE : 타입 선언
- 
###### @Inherited
어노테이션의 상속을 가능하게 합니다.

###### @Repeatable
Java8 부터 지원하며, 연속적으로 어노테이션을 선언할 수 있게 해줍니다.


#### 사용예제
@Override
```
public class Animal {
    public void speak() {
    }

    public String getType() {
        return "Generic animal";
    }
}

public class Cat extends Animal {
    @Override
    public void speak() { // This is a good override.
        System.out.println("Meow.");
    }

    @Override
    public String gettype() { // Compile-time error due to mistyped name.
        return "Cat";
    }
```

#### 커스텀 어노테이션
커스텀 어노테이션을 이용하는 방법
1. 어노테이션을 정의한다.
2. 어노테이션을 클래스에서 사용한다. (타겟에 적용)
3. 어노테이션을 이용하는 코드를 수행한다.

```
// 어노테이션 생성
	import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;

    @Retention(RetentionPolicy.RUNTIME)	// 런타임중에도 유효한 어노테이션임을 기술
    public @interface Count100 {	// 어노테이션은 @interface 인터페이스명으로 정의
	
    }
```

```
// 커스텀 어노테이션을 메소드에 적용
	public class MyHello {
        @Count100
        public void hello(){
            System.out.println("hello");
        }
    }
```
 
```
// 어노테이션이 적용된  부분인지 체크하여 코드내에서 사용

    import java.lang.reflect.Method;

    public class MyHelloExam {
        public static void main(String[] args) {
            MyHello hello = new MyHello();

            try{
                Method method = hello.getClass().getDeclaredMethod("hello");
            if(method.isAnnotationPresent(Count100.class)){
                    for(int i = 0; i < 100; i++){
                        hello.hello();
                    }
                }else{
                    hello.hello();
                }
            }catch(Exception ex){
                ex.printStackTrace();
            }       
        }
    }
```
