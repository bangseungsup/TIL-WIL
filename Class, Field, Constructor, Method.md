### 클래스
- 객체를 생성하기 위한 설계도
- 클래스는’사용자정의 자료형’이라고 생각할 수 있다.
- 클래스는 객체를 생성하기 위한 필드와 메소드의 집합이다.
- Object.class 는 모든 객체의 부모객체이다.
- 클래스와 클래스의 관계는 has a 관계, is a 관계 로 설명 가능하다.
- has a 관계 : 포함 관계 / car has a Tire, car has a Engine
- is a 관계: 상속관계, 구현관계 / Galaxy is a smartphone, Iphone is a smarthone


## 필드
- 고유 데이터, 현재 상태정보(속성), 다른 객체를 저장
- 변수와 비슷한 형태로 정의한다.
- 변수는 ‘선언 위치’에 따라 필드 / 지역변수, 매개변수 로 나뉜다.
  - 지역변수(Local variable): **메소드 안에서 선언한 변수**
  - 선언한 메소드 안에서만 사용 가능하다.
  - 메소드의 실행이 종료되면 즉시 사라진다.
  - 변수가 자동으로 초기화되지 않기 때문에 반드시 초기화한 후에 사용해야 한다.
  - 필드(Field) : **메소드 밖, 클래스에서 선언한 변수**
    프로퍼티, 멤버변수, 인스턴스변수 모두 필드를 의미하는, 같은 말이다.
    
    > 멤버변수 : 객체 생성에 참여하는 변수. 생성된 객체에 포함되는 변수. 라는 의미
    > 인스턴스변수 : 객체 생성하면 사용할 수 있는 변수. 라는 의미


## 생성자
- 클래스의 이름과 동일한 이름을 가진 메소드
- 반환 타입이 없다. (void도 아님, 반환타입 자체가 없는 것. 반환타입이 있을 경우 생성자메소드가 아니다.)
- 기능
    - 객체 생성 시 초기화 작업을 수월하게 해준다.
    - 객체가 생성된 후 자동으로 실행된다 ⇒ 객체 생성 후 처음으로 실행할 작업을 생성자 메소드로 구현한다.
    - 첫 작업은 대부분
      - 멤버변수를 초기화하는 작업
      - 외부자원(네트워크, 데이터베이스 등)과 연결하는 작업이다.
- 사용
    -  객체를 생성할 때 new 키워드 다음에 적는다. Person p1 = new Person();
    -  참조변수를 통해 실행할 수 없다. 참조변수.생성자메소드(); ⇒ 실행불가.
      - 객체를 생성할 때 = 객체와 참조변수를 연결할 때 생성자메소드를 이미 실행하니까 말이 안됨.
    - 클래스에 생성자메소드가 없으면 컴파일러가 ‘기본 생성자 메소드’를 자동으로 추가한다. public Person() { } 형식으로 생성된다.
      - 기본 생성자 메소드: 매개변수가 없는 생성자 메소드
    - 생성자도 중복정의가 가능하다
    - this 키워드가 함께 활용된다.

## 메소드
- 객체의 기능에 해당하는, 수행문의 코드블록
- 선언부와 구현부로 구성된다.
    - 선언부 : 접근제한자 반환타입 메소드이름(타입 매개변수명, 타입 매개변수명..)
    - 구현부: {해당 메소드가 수행하는 작업에 대한 수행문이 포함됨}
      - 매개변수 : 메소드 실행에 필요한 값을 전달 받아서(=인자값) 저장하는 변수
      - 반환타입 : 메소드를 실행하면 획득하게 되는 값(변수)의 타입. 반환값이 없을 경우 void
      - 메소드이름 은 동사형으로 붙여준다.
      - 입출력은 각각 있을 수도, 없을 수도 있으며 선언부, 구현부를 보고 알 수 있다.
