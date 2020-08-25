# 즉시로딩 (Eager Loading)

### 1. 사용방법
@ManyToOne의 fetch 속성을 FetchType.EAGER로 지정


### 2. NULL 제약조건과 JPA 조인 전략
회원을 조회할 때 해당 회원의 팀 정보를 같이 보고자 한다면 JPA구현체는 즉시로딩을 최적화하기 위해 가능하면 조인쿼리를 사용한다.

JPA는 테이블의 외래키의 NULL값 여부에 따라 외부(Outer) 조인, 내부(Inner) 조인을 사용한다.


** nullable 설정에 따른 조인 전략 **
- @JoinColumn(nullable = true) : NULL 허용(기본값), 외부 조인 사용
- @JoinColumn(nullable = false) : NULL 허용하지 않음, 내부 조인 사용

위의 조인전략을 정리하자면 JPA는 선택적 관계면 외부 조인을 사용하고 필수 관계면 내부 조인을 사용한다.

### 3. 컬렉션에 사용 시 주의점
1. 컬렉션을 하나 이상 즉시 로딩하는 것은 권장하지 않는다.
2. 컬렉션 즉시 로딩은 항상 외부 조인(Outer Join)을 사용한다.

## FetchType.EAGER 설정과 조인 전략

### @ManyToOne, @OneToOne

- (optional = false): 내부 조인
- (optional = true): 외부 조인

### @OneToMany, @ManyToMany

- (optional = false): 외부 조인
- (optional = true): 내부 조인

# 지연로딩 (Lazy Loading)

### 1. 사용방법
@ManyToOne의 fetch 속성을 FetchType.LAZY로 지정


### 2. 특징
회원과 팀을 지연로딩으로 설정했을때 회원 조회시 팀은 조회하지 않는다.
대신에 팀에 해당되는 프록시 객체를 넣어두고 실제 사용될 때까지 데이터 로딩을 미루어 지연로딩이라고 한다.

조회 대상이 영속성 컨텍스트에 이미 있으면 프록시 객체를 사용할 이유가 없어 프록시가 아닌 실제 객체를 사용한다.
예를 들어 team1 엔티티가 영속성 컨텍스트에 이미 로딩되어 있으면 프록시가 아닌 실제 team1 엔티티를 사용한다.

# EagerLoading vs LazyLoading
Eager: 연관된 엔티티를 즉시 조회한다. 하이버네이트는 가능하면 SQL조인을 사용해서 한 번에 조회한다. @ManyToOne, @OneToOne

Lazy: 연관된 엔티티를 프록시로 조회한다. 프록시를 실제 사용할 때 초기화하면서 데이터베이스를 조회한다. @OneToMany, @ManyToMany

JPA의 기본 Fetch 전략은 연관된 엔티티가 하나면 즉시 로딩, 컬렉션이면 지연 로딩을 사용.
컬렉션을 로딩하는 것은 비용이 많이 들고 잘못하면 너무 많은 데이터를 로딩할 수 있기 때문.
** 추천하는 방법은 모든 연관관계에 지연 로딩을 사용하는 것이다. **
 