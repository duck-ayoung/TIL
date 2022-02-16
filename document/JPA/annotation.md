## annotation

@Inheritance(strategy=InheritanceType.XXX)
> 상속관계를 객체를 table에 매핑시킬 때 사용하는 annotation
> <br> 해당 annotation 을 부모 클래스에 선언해준다.

<br> **사용할 수 있는 전략**
- InheritanceType.JOINED: 조인 전략 (부모 table과 자식 table을 만들고 자식 table에 외래키를 설정하여 JOIN하여 사용하는 전략)
- InheritanceType.SINGLE_TABLE: 단일 테이블 전략 (부모 table안에 자식 객체의 속성을 모두 포함하는 전략)
- InheritanceType.TABLE_PER_CLASS: 구현 클래스마다 테이블 전략 (자식 table안에 부모 객체의 속성을 모두 포함하는 전략)

@DiscriminatorColumn(name=“DTYPE”)
> 하위 클래스를 구분하는 용도의 컬럼을 추가하고 정보를 셋팅하는 annotation
> <br> 해당 annotation 을 부모 클래스에 선언해준다.

@DiscriminatorValue(“XXX”)
> 자식 클래스에 정의 위의 DiscriminatorColumn 컬럼의 속성 값을 설정할 수 있다 (기본은 클래스 이름)

@MappedSuperclass
> 공통 매핑 정보가 필요할 때 사용하는 annotation, table이 생성되는 것이 아니라 자식 클래스에 매핑 정보만 제공한다.
> <br> 해당 annotation 을 부모 클래스에 선언해준다.

연관관계 매핑
1. @ManyToOne 
2. @OneToMany
3. @OneToOne
4. 속성
<br> fetch : FetchType 설정 (LAZY or EAGER)

<br> 값 타입
<br> @Embeddable 
> 값 타입을 정의하는 곳에 선언

<br> @Embedded 
> 값 타입을 사용하는 곳에 표시

<br> @AttributeOverride
> 한 엔티티에서 같은 값 타입을 사용할 때 표시(컬럼명의 재정의)

<br> @ElementCollection
> 값 타입의 컬렉션이 필요할 때 사용
> <br> @CollectionTable을 사용해 테이블 명 
> <br> 변경하면 추적이 어렵고, 변경이 발생하면 주인 엔티티와 관련된 모든 데이터를 삭제하고 다시 insert하기 때문에(값 타입 테이블은 명확한 PK가 없기 때문에 발생하는 일) **사용하면 안된다.**
