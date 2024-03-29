### Index
RDBMS에서 검색속도를 높이기 사용하는 하나의 기술. 하나 TABLE의 컬럼을 색인화(따로 파일로 저장)하여 검색시 해당 TABLE의 레코드를 full scan 하는게 아니라 색인화 되어 있는 
INDEX 파일을 검색하여 **검색속도를 빠르게 합니다.** INDEX는 Tree구조로 색인화되어 있으며, RDBMS에서 사용하는 INDEX는 Balance Search Tree를 사용합니다.


#### INDEX의 원리
INDEX를 해당 컬럼에 주게 되면 초기 TABLE 생성 시, 만들어진 MYD(MySQL Data), MYI(MySQL Index), FRM(Format) 3개의 파일중에서 MYI에 해당 컬럼을 색인화하여 저장합니다. Index를 사용하지 않을 땐 
MYI는 비어 있으며, INDEX를 어떤 컬럼에 지정하면 해당 컬럼을 따로 인덱싱하여 MYI 파일에 입력합니다. 

사용자가 SELECT 쿼리로 INDEX가 지정된 TABLE 조회 시, 빠른 TREE로 정리해 둔 MYI 파일의 내용부터 검색합니다. 
만약 INDEX를 지정하지 않은 테이블을 조회하는 SELECT 쿼리라면 해당 TABLE을 full scan하여 모두 검색합니다.

##### INDEX 장점
- 키 값을 기초로 하여 테이블에서 검색과 정렬 속도를 향상
- 질의나 보고서에서 그룹화 작업의 속도를 향상
- 인덱스를 사용하면 테이블 행의 고유성을 강화시킬 수 있음
- 테이블의 기본 키는 자동으로 인덱스 됨


##### INDEX 단점
- 인덱스를 만들면 .mdb 파일 크기가 증가
- 여러 사용자, 여러 응용 프로그램에서 한 페이지를 동시에 수정할 수 있는 병행성 감소
- 인덱스 된 필드에서 데이터를 업데이트하거나, 레코드를 추가 또는 삭제할 때 성능 감소
- 인덱스가 데이터베이스 공간을 차지해 추가적인 공간 필요 (DB 10% 내외의 공간 추가 필요)
- 인덱스를 생성하는데 시간 소요
- 데이터 변경 작업이 자주 일어날 경우에 인덱스를 재작성해야 할 필요가 있기에 성능에 영향을 끼칠 수 있음

**따라서 가장 고유한 값을 갖는 필드만 Index 하는 것이 중요**


#### INDEX 목적
인덱스의 목적은 해당 RDBMS의 **검색 속도를 높이는 것**이 주 목적이다.SELECT 쿼리의 WHERE절이나 HOIN 예약어를 사용했을때만 인덱스를 사용하게 되며 SELECT 쿼리의 검색 속도를 바르게 하는게 목적이다.

DELETE, INSERT, UPDATE 쿼리에는 해당 사항이 없고, INDEX 사용 중인 경우는 느려진다.


#### INDEX 주의할 점
- 인덱스는 열 단위로 생성
- WHERE 절에서 사용되는 컬럼을 인덱스로 만듬
- 데이터의 중복도가 높은 열은 인덱스로 만들어도 효용이 없음
- 외래키가 사용되는 열에는 인덱스를 되도록 생성해주는 것이 좋음
- JOIN에 자주 사용되는 열에는 인덱스를 생성햊는 것이 좋음
- 사용하지 않는 인덱스는 제거 필요


### 복합 인덱스
두 개 이상의 컬럼을 합쳐서 인덱스를 만드는 것을 말한다. 주로 단일 컬럼에서는 나쁜 분포도를 가지지만 여러 개의 컬럼을 합친다면 좋은 분포도를 가지고, WHERE절에서 AND 조건에 많이 사용되는 컬럼들을 결합 인덱스로 구성한다.

> 결합 인덱스의 컬럼 결정
결합 인덱스를 만들 때 결합 인덱스를 구성하는 컬럼들의 배열 순서는 아주 중요하기에 신중하게 결정해야 한다. 컬럼의 순서를 잘못 배열하면 결합 인덱스의 발동 확률이 매우 낮아질 수 있기 때문이다.

만약 select문의 where절에 결합 인덱스의 첫 번째 컬럼을 조건에 사용하였다면 그 질의문은 결합 인덱스를 사용할 수 있다. 하지만 개발자가 결합 인덱스의 두번째 컬럼만을 where절에 조건으로 사용하고 결합 인덱스를 사용하고자 했다면 실행계획은 인덱스를 사용하지 못한다.

**따라서 쿼리문 작성 시 결합 인덱스를 사용하고자 한다면 반드시 결합 인덱스의 컬럼 중 선행하는 컬럼부터 조건에 지정하여 사용**하여야 한다. 조건은 컬럼 전체를 순서대로 사용할 수 있고, 아니면 선행하는 일부 컬럼을 순서대로 사용할 수 있다.

> 결합 인덱스 컬럼 설정 시 고려해야 할 우선 순위
> 1. WHERE절 조건에 많이 사용되는 컬럼이 우선 시
> 2. Equal('=')로 사용되는 컬럼 우선
> 3. 분포도가 좋은 컬럼을 우선
> 4. 자주 이용되는 순서대로 결합 인덱스 컬럼의 순서 결정

1. 조건절에서 첫 번째 컬럼을 조건에서 사용하지 않는다면, 그 인덱스는 사용되지 않는 경우가 대부분이다. 그렇기에 많은 쿼리에서 공통적으로 사용된 조건절의 컬럼을 인덱스 선행 컬럼에 주로 사용한다.

2. 결합 인덱스에서 선행 컬럼이 '=' 조건이 아니라면 후행 컬럼 조건에서 '='을 사용하더라도 처리 범위는 줄어들지 않는다. **조건절에서 '='이 아닌 사용하는 첫 번째 컬럼까지만 인덱스를 타고 그 이후는 인덱스를 타지 않고 필터 즉, 체크만 한다.**
```
WHERE 컬럼1 = ?
AND   컬럼2 = ?
AND   컬럼3 BETWEEN ? AND ? -- 결합인덱스에서 '='이 아닌 연산자를 사용하는 첫 번째 
AND   컬럼4 = ?
```
결합 인덱스 = 컬럼1 + 컬럼2 + 컬럼3 + 컬럼4 라고 할 때, 컬럼3에서 BETWEEN을 사용했기에 컬럼3까지만 인덱스를 타고 후행컬럼인 컬럼4는 인덱스를 타지 않고 필터만 한다. 

그래서 결합인덱스 = 컬럼1 + 컬럼2 + 컬럼4 + 컬럼3으로 순서를 바꿔줘야 한다.

3. 흔히 분포도가 좋은 컬럼이 처리 범위를 줄여주므로 결합 인덱스의 선행컬럼으로 해줘야 한다. 하지만 굳이 분포도가 좋은 컬럼이면 단일 인덱스로서 사용되면 되고 굳이 결합인덱스로 사용할 필요가 없다. 결합인덱스를 사용하는 이유중 하나가 하나의 컬럼만으로는 분포도가 좋지않지만 여러개의 컬럼으로 분포도를 향상시켜 처리 범위를 줄여주는데에 있기 때문이다. 



