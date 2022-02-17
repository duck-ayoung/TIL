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
