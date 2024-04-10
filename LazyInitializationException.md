##### 발생 상황

: 댓글을 저장하는 테스트 환경에서 댓글에 회원, 게시판 엔티티를 저장하는 도중, 에러가 발생했습니다.

```
org.hibernate.LazyInitializationException: 
failed to lazily initialize a collection of role
: com.example.demo.question.entity.Inquiry.iCommentList, 
could not initialize proxy - no Session
```

##### 발생 원인 

: findById를 해서 찾은 객체는 Lazy 로딩을 사용했기 때문에  실제 객체가 아닌 프록시 객체입니다. 위 no Session은 영속성 컨텍스트에 내부라고 생각하면 세션에 없기 때문에 준영속성 상태이기 때문에 댓글에서 회원, 게시판 객체가 저장이 되지 않았던 것입니다.

<br>

##### 해결방법

1. OSIV 켜기
2. `@Transactional` 사용하기

<br>

##### OSIV(Open-Session-In-View)

: Hibernate에서는 Open Session In View라 하고, JPA에서는 Open EntityManager In View라 하는데 관례상 OSIV라 합니다. OSIV(Open Session In View)는 영속성 컨텍스트를 컨트롤러와 뷰까지 열어두는 기능으로 영속성 컨텍스트가 살아있다면 컨트롤러와 뷰에서도 지연 로딩을 사용할 수가 있습니다. 기본이 true입니다. 

스프링 OSIV - true

Spring Boot Jpa 에서 의존성을 주입 받아 어플리케이션을 구성할 경우 spring.jpa.open-in-view의 기본 값인 true로 지정되어 있어 OSIV가 적용된 상태로 어플리케이션이 구성됩니다

```
spring.jpa.open-in-view=true
```

#### 영속성 범위(true)
![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/3f0db08f-ba29-4ed5-9375-2dfed5c8d341)

+ 동작 

1. 클라이언트의 요청이 들어오면 서블릿 필터나, 스프링 인터셉터에서 영속성 컨텍스트를 생성합니다. 단 이 시점에서 트랜잭션은 시작하지 않습니다.

2. 서비스 계층에서 @Transeactional로 트랜잭션을 시작할 때 1번에서 미리 생성해둔 영속성 컨텍스트를 찾아와서 트랜잭션을 시작합니다.

3. 서비스 계층이 끝나면 트랜잭션을 커밋하고 영속성 컨텍스트를 플러시합니다. 이 시점에 트랜잭션은 끝내지만 영속성 컨텍스트는 종료되지 않습니다.

4. 컨트롤러와 뷰까지 영속성 컨텍스트가 유지되므로 조회한 엔티티는 영속 상태를 유지합니다

5. 서블릿 필터나, 스프링 인터셉터로 요청이 돌아오면 영속성 컨텍스트를 종료합니다. 이때 플러시를 호출하지 않고 바로 종료합니다.

→ 비즈니스 계층에서 트랜잭션이 끝났기 때문에 컨트롤러 계층에서 영속 상태의 엔티티는 수정이 가능(OSIV)하지만 데이터베이스에는 반영되지 않습니다. 

+ 특징

- 클라이언트 요청이 끝날 때까지 영속 상태 유지합니다.
- 엔티티 수정은 `@Transactional` 이 있는 계층만 동작, 없는 계층에서는 지연 로딩을 포함해서 조회만 가능합니다.

+ 단점 

- 컨트롤러에서 수정하고 비즈니스 계층에서 `@Transactional`할 경우 엔티티가 수정될 수 있음
- OSIV를 적용하면 같은 영속성 컨텍스트를 여러 트랜잭션이 공유할 수 있게 됩니다.
- DB 커넥션이 몰릴게 될 수 있습니다.

#### 스프링 OSIV - false & `@Transactional` 사용

```
spring.jpa.open-in-view=false
```

영속성 범위(false)
![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/3f0db08f-ba29-4ed5-9375-2dfed5c8d341)


동작

1. 서비스 계층에서 @Transeactional로 트랜잭션을 시작할 때 영속성 컨텍스트를 생성합니다. 

2. 서비스 계층이 끝나면 트랜잭션을 커밋하고 영속성 컨텍스트를 플러시하고 트랜잭션,영속성 컨텍스트를 종료합니다.

3. DB Connection 반환합니다.

+ 장점

: DB 커넥션 리소스의 효율적으로 사용할 수 있습니다. 

+ 단점

: LazyInitializationException 발생 가능성이 높아지고 예외를 처리하기 위해 서비스 계층에서 모든 로직을 처리하도록 구현하기 때문에 코드가 지저분해지고 유지보수가 떨어집니다. 

LazyInitializationException → 해결하기 위한 방법은 한 트랜잭션 안에서 미리 로딩을 하거나 Fetch Join또는 @EntityGraph를 사용해서 조회할 엔티티 조회 후, 관련 엔티티도 함께 조회하는 것입니다. 

+ OSIV - 대안

JPQL을 사용해서 엔티티와 가장 유사한 DTO룰 사용해서 컨트롤러 계층에 반환하는 것입니다. 하지만 많은 코드를 작성해야 하는 단점이 존재합니다.   

실무에서는 웬만하면 끄고 사용하는 것이 좋습니다.

