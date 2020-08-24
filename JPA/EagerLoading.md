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