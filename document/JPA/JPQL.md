## JPQL
JPQL 
- Java Persistence Query Language
- JPA에서 제공하는 쿼리 방법으로 SQL을 추상화한 객체지향 쿼리언어이다.
- 객체를 대상으로 쿼리를 작성한다.
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.

```java
/*
아래처럼 했을때 memberA와 memberB만 조회된다.
*/
Member memberA = new Member();
memberA.setUsername("memberA");
em.persist(memberA);
Member memberB = new Member();
memberB.setUsername("memberB");
em.persist(memberB);
Member memberC = new Member();
memberC.setUsername("hi");
em.persist(memberC);

List<Member> resultList = em.createQuery("select m From Member m where m.userName like 'member%'", Member.class)
							.getResultList();
```

문법

- 엔티티와 속성은 대소문자 구분한다.
- 키워드는 대소문자 구분하지 않는다.
- 엔티티 이름을 사용한다. 테이블 이름이 아니다.
- 쿼리의 타입이 무엇인지 지정할 수 있다. createQuery 두번째 파라미터에 지정.

```java
TypedQuery<Member> query1 = em.createQuery("select m From Member m", Member.class);
Query query2 = em.createQuery("select m From Member m");
```

- 파라미터 바인딩 :파라미터이름 ← 쿼리 작성후 setParameter로 바인딩 해준다.
```java
TypedQuery<Member> query = em.createQuery("select m From Member m where m.username =:username", Member.class);
query.setParameter("username", "memberA");

List<Member> resultList = query.getResultList();
```
- 조회할 정보가 하나의 타입으로 정의할 수 없는 경우는 Object로 받는다.

```java
List<Object> resultList = em.createQuery("select m.username, m.age, m.team From Member m")
                    .getResultList();

Object[] objects = (Object[]) resultList.get(0);

System.out.println("member.getUsername() = " + objects[0]);
System.out.println("member.getAge() = " + objects[1]);
System.out.println("member.getTeam() = " + objects[2]);
```

페이징 API

- setFirstResult(int startPosition) : 조회 시작 위치
- setMaxResult(int maxResult) : 조회할 데이터 수


조인

- 관계형 데이터베이스에서 중복 데이터를 피하기 위해서 데이터를 쪼개 여러 테이블로 나누어서 저장하는데, 쪼개진 테이블을 조합하여 조회하고 싶은 경우에 JOIN 연산자를 사용해 컬럼 기준으로 행을 합쳐줄 수 있다.
    
    ```java
    // JOIN 키워드로 쿼리를 작성하면 JPA가 아래 주석처럼 inner join으로 sql문을 생성하여 준다.
    
    List<Member> resultList = em.createQuery("SELECT m From Member m JOIN m.team t", Member.class)
                        .getResultList();
    
    /**
    select
    	member0_.id as id1_0_,
    	member0_.age as age2_0_,
    	member0_.TEAM_ID as TEAM_ID4_0_,
    	member0_.username as username3_0_ 
    from
    	Member member0_ 
    inner join
    	Team team1_  on member0_.TEAM_ID=team1_.TEAM_ID
    **/
    ```
    

- ON  (필터링)
    
    ```java
    List<Member> resultList = em.createQuery("SELECT m From Member m JOIN m.team t on t.name ='A' ", Member.class)
                        .getResultList();
    ```
    

서브 쿼리

- 메인 쿼리랑 서브 쿼리는 독립적으로 짜야 성능 잘 나옴
    
    ```java
    //메인 쿼리의 m을 서브 쿼리에서 사용하지 않고 m2로 따로 관리
    List<Member> resultList = em.createQuery("SELECT m From Member m where m.age > (select avg(m2.age) from Member m2)", Member.class)
            .getResultList();
    ```
    
- Exists : 서브쿼리의 조건에 맞는 행만 추출된다.
    
    ```java
    List<Member> resultList = em.createQuery("SELECT m From Member m where exists (select t from m.team t where t.name='teamA')", Member.class)
                        .getResultList();
    ```
    
- ANY
    
    ```java
    List<Member> resultList = em.createQuery("SELECT m From Member m where m.team = ANY (select t from Team t)", Member.class)
                        .getResultList();
    ```

CASE 식

```java
List<String> resultList = em.createQuery("select "
																							+ "case b.name "
																								+ "when 'JPA' then '베스트셀러' " +
															                    "else '일반도서' " +
														                    "end " +
			                    "from Book b", String.class).getResultList();
```

COALESCE : 하나씩 조회해서 null이 아니면 반환 null 이면 두번째 파라미터로 설정해준 값 반환

```java
List<String> resultList =
        em.createQuery("select coalesce(b.name, '이름 없는 책') from Book b", String.class).getResultList();

```

JPQL 기본 함수

- CONCAT, SUBSTRING, TRIM, LOWER, UPPER, LOCATE, ABS, SQRT, MOD, SIZE, INDEX ⇒  JPQL이 제공하는 표준함수, 데이터베이스에 관계없이 쓰면 된다.

사용자 정의 함수 호출

1. `Dialect` 만들기
    - 내가 사용하는 diarect 를 상속받아 새로운 diarect를 만들어준다.
    
    ```java
    public class MyH2Dialect extends H2Dialect {}
    ```
    
2. 함수 등록하기
    - 등록하고자하는 함수를 등록해준다, 등록하는 방법은 상속받은 diarect 클래스에서 참조하기
    
    ```java
    public MyH2Dialect() {
            registerFunction("group_concat", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));
        }
    ```
    
3. 설정을 바꾸어준다.
    
    ```java
    //persistence.xml
    <property name="hibernate.dialect" value="dialect.MyH2Dialect"/>
    ```
    
4. 등록한 함수를 사용한다
    
    ```java
    List<String> resultList =
                        em.createQuery("select function('group_concat', b.name) from Book b", String.class).getResultList();
    ```
    
