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
