### Fetch Type
Fetch Type이란 JPA가 하나의 Entity를 조회할 때, 연관 관계에 있는 객체들을 어떻게 가져올 것인지 나타내는 설정 값
- JPA는 ORM 기술로 사용자가 직접 쿼리를 생성하지 않고, JPA에서 JPQL을 이용하여 쿼리문을 생성, 객체와 필드를 보고 쿼리 생성
- 다른 객체와 연관 관계 맵핑이 되어 있다면 그 객체들까지 조회하게 되는데, 이때 이 객체를 어떻게 불러올 것인가를 설정할 수 있음

### 즉시로딩 Eager
즉시로딩이란 데이터를 조회할 때, 연관된 모든 객체의 데이터까지 한꺼번에 불러오는 것을 말합니다.

> member entity에 team entity가 N:1 맵핑으로 관계 설정
```
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;
    private String username;

    @ManyToOne(fetch = FetchType.EAGER) //Team을 조회할 때 즉시로딩을 사용
    @JoinColumn(name = "team_id")
    Team team;
}

@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;
    private String teamname;
}
```

JPQL로 member 조회 시,
```
Member findMember = em.createQuery("select m from Member m", Member.class).getSingleResult();
```
위와 같이 SQL 쿼리문으로 조회 진행
```
//멤버를 조회하는 쿼리
select
    member0_.id as id1_0_,
    member0_.team_id as team_id3_0_,
    member0_.username as username2_0_ 
from
    Member member0_

//팀을 조회하는 쿼리
select
    team0_.id as id1_3_0_,
    team0_.name as name2_3_0_ 
from
    Team team0_ 
where
    team0_.id=?
```
다음과 같이 즉시 로딩(EAGER) 방식을 사용하면 Member를 조회하는 시점에 바로 Team까지 불러오는 쿼리를 날려 한꺼번에 데이터를 불러오는 것을 볼 수 있습니다.

------

### 지연로딩 Lazy
지연로딩이란 필요한 시점에 연관된 객체의 데이터를 불러오는 것을 말합니다.

```
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;
    private String username;

    @ManyToOne(fetch = FetchType.Lazy) //Team을 조회할 때 지연로딩을 사용
    @JoinColumn(name = "team_id")
    Team team;
}

@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;
    private String teamname;
}
```
JPQL로 member 조회 시, 
```
Member findMember = em.createQuery("select m from Member m", Member.class).getSingleResult();
```
실제 이렇게 SQL 쿼리문

```
//Team을 조회하는 쿼리가 나가지 않음!
select
    member0_.id as id1_0_,
    member0_.team_id as team_id3_0_,
    member0_.username as username2_0_ 
from
    Member member0_
```

----------

즉시로딩 형태로 member 조회 시, 연관된 객체인 team까지 조회하는 쿼리문이 생성되었지만,
지연로딩 형태로 member 조회 시, 지금 필요하지 않은 team은 조회하지 않고 member만 조회하는 쿼리가 생성이 됩니다.

연관 관계가 많아질수록, 즉시로딩 형태로 조회 시 필요하지 않은 데이터까지 조회가 되기 때문에 성능 저하를 포함한 비효율적인 모습을 보일 수 있습니다.
