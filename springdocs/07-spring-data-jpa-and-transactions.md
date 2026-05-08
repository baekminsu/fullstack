# Spring Data JPA와 트랜잭션

JPA(Java Persistence API)는 Java 객체와 관계형 데이터베이스 테이블을 매핑하는 표준입니다. Hibernate는 JPA의 대표 구현체입니다. Spring Data JPA는 반복적인 리포지토리 코드를 줄여주는 Spring 프로젝트입니다.

## Entity

Entity는 DB(Database) 테이블과 매핑되는 객체입니다.

```java
@Entity
@Table(name = "users")
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String email;

    protected UserEntity() {
    }

    public UserEntity(String email) {
        this.email = email;
    }
}
```

JPA는 프록시와 리플렉션을 위해 기본 생성자를 필요로 합니다. 외부에서 잘못 쓰지 않도록 `protected`로 둡니다.

## Repository

```java
public interface UserJpaRepository extends JpaRepository<UserEntity, Long> {
    Optional<UserEntity> findByEmail(String email);
    boolean existsByEmail(String email);
}
```

메서드 이름으로 쿼리를 만들 수 있지만, 복잡한 조건은 명시적 쿼리나 QueryDSL 같은 도구를 고려합니다.

## 영속성 컨텍스트

영속성 컨텍스트는 Entity를 관리하는 1차 캐시이자 변경 감지 공간입니다.

```java
@Transactional
public void changeEmail(Long userId, String email) {
    UserEntity user = userRepository.findById(userId)
        .orElseThrow(() -> new UserNotFoundException(userId));
    user.changeEmail(email);
}
```

`save`를 다시 호출하지 않아도 트랜잭션 커밋 시점에 변경 감지가 일어나 update SQL(Structured Query Language)이 실행됩니다.

## 트랜잭션

트랜잭션은 여러 DB 작업을 하나의 작업 단위로 묶습니다.

ACID(Atomicity, Consistency, Isolation, Durability):

- Atomicity: 모두 성공하거나 모두 실패합니다.
- Consistency: 제약 조건과 규칙이 깨지지 않습니다.
- Isolation: 동시에 실행되는 트랜잭션이 서로를 얼마나 볼 수 있는지 정합니다.
- Durability: 커밋된 결과는 보존됩니다.

Spring에서는 서비스 유스케이스 단위에 `@Transactional`을 둡니다.

```java
@Transactional
public Long placeOrder(PlaceOrderCommand command) {
    User user = userRepository.getById(command.userId());
    Product product = productRepository.getById(command.productId());
    product.decreaseStock(command.quantity());
    Order order = Order.place(user, product, command.quantity());
    orderRepository.save(order);
    return order.getId();
}
```

## 읽기 전용 트랜잭션

```java
@Transactional(readOnly = true)
public UserResponse getUser(Long userId) {
    return userRepository.findById(userId)
        .map(UserResponse::from)
        .orElseThrow(() -> new UserNotFoundException(userId));
}
```

조회 메서드는 `readOnly = true`를 붙여 의도를 드러냅니다.

## 연관관계

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "user_id", nullable = false)
private UserEntity user;
```

기본은 지연 로딩을 선호합니다. 즉시 로딩은 예상하지 못한 쿼리를 만들기 쉽습니다.

양방향 연관관계는 꼭 필요할 때만 둡니다. 양방향이면 연관관계 편의 메서드를 만들어 양쪽 상태를 함께 맞춥니다.

```java
public void addComment(CommentEntity comment) {
    comments.add(comment);
    comment.setPost(this);
}
```

## N+1 문제

N+1 문제는 목록 하나를 조회한 뒤 각 항목의 연관 데이터를 추가 쿼리로 조회해 쿼리가 폭증하는 문제입니다.

해결 방법:

- fetch join
- EntityGraph
- DTO(Data Transfer Object) 직접 조회
- batch size 설정

예시:

```java
@Query("select p from PostEntity p join fetch p.author where p.id = :id")
Optional<PostEntity> findByIdWithAuthor(@Param("id") Long id);
```

## 페이지네이션

```java
Page<PostEntity> page = postRepository.findAll(PageRequest.of(0, 20));
```

대량 데이터에서는 offset pagination이 느려질 수 있습니다. 무한 스크롤이나 최신순 목록은 keyset pagination을 고려합니다.

```sql
select *
from posts
where id < :lastSeenId
order by id desc
limit 20;
```

## 낙관적 잠금

```java
@Version
private Long version;
```

낙관적 잠금은 충돌이 적다는 가정에서 version으로 동시 수정을 감지합니다. 재고 차감, 쿠폰 사용처럼 충돌 가능성이 있는 곳에서 검토합니다.

## 체크리스트

- [ ] JPA(Java Persistence API), Hibernate, Spring Data JPA의 관계를 설명할 수 있다.
- [ ] 영속성 컨텍스트와 변경 감지를 설명할 수 있다.
- [ ] 트랜잭션 경계를 유스케이스 기준으로 잡는다.
- [ ] N+1 문제를 실행 SQL로 확인할 수 있다.
- [ ] 페이지네이션 방식의 성능 차이를 이해한다.
