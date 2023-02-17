## JPA (Java Persistence API)
JPA는 자바 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용되는 인터페이스 모음이다. 실제적으로 구현된 것이 아니라 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이다.

-------
#### ORM (Object-Relational Mapping)
우리가 일반적으로 알고 있는 어플리케이션 class와 RDB(Realational Database)의 테이블을 맵핑(연결)한다는 뜻이며, 기술적으로는 어플리케이션 객체를 RDB 테이블에 자동으로 영속화 해주는 것이라고 보면 된다.

###### 장점
- SQL문이 아닌 method를 통해 DB를 조작할 수 있어, 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있음 (내부적으로는 쿼리르 생성하여 DB를 조작하지만 개발자는 신경쓰지 않아도 된다.)
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높인다.
- 객체지향적인 코드 작성이 가능하다. 오직 객체지향적 접근만 고려하면 되기 떄문에 생산성이 증가한다.
- 맵핑하는 정보가 class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리하다.
- DB 변경 시 새로 쿼리를 짜야하는 경우가 생기지만, 이런 경우 ORM을 사용한다면 쿼리를 수정할 필요가 없다.

###### 단점
- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 발생할 수 있다.
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있다.
- 학습 비용이 비싸다.

-------
### JPA
- Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음이다.
- 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스이다.
- 인터페이스이기 때문에 Hibernate, Open JPA 등이 JPA를 구현한다.

###### 왜 JPA를 사용해야 할까?
JPA는 반복적인 CRUD SQL을 처리해준다. JPA는 맵핑된 관계를 이용해서 SQL을 생성하고 실행하는데, 개발자는 어떤 SQL이 실행될지 생각만하면 되고, 예측도 쉽게 할 수 있다.추가적으로 JPA는 네이티브 SQL이란 기능을 제공해주는데 관계 맵핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL을 직접 작성하여 사용할 수 있다.

JPA를 사용하여 얻을 수 있는 가장 큰 것은 SQL이 아닌 객체 중심으로 개발할 수 있다는 것이다. 이에 따라 당연히 생산성이 좋아지고 유지보수도 수월하다. 또한 JPA는 패러다임의 불일치도 해결한다.

Java의 상속 관계의 경우, 데이터베이스에서는 이러한 객체의 상속 관계를 지원하지 않지만, JPA에서는 지원이 가능하다.

![image](https://user-images.githubusercontent.com/118147296/219601874-e6e064c9-13f7-47cb-871a-bb91d8928988.png)
출처 : https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html

![image](https://user-images.githubusercontent.com/118147296/219601919-f1c5f34d-76cc-4311-966a-60697fd9422e.png)
출처 : https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html

