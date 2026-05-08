# 보안, 테스트, 관측 가능성

백엔드 개발은 기능 구현에서 끝나지 않습니다. 인증, 권한, 테스트, 로깅, 메트릭이 있어야 운영 가능한 서버가 됩니다.

## 인증과 인가

인증(Authentication)은 사용자가 누구인지 확인하는 일입니다. 인가(Authorization)는 그 사용자가 어떤 행동을 할 수 있는지 판단하는 일입니다.

예시:

- 로그인 성공: 인증
- 관리자 페이지 접근 허용: 인가

두 개념을 섞으면 코드가 복잡해지고 보안 구멍이 생깁니다.

## Spring Security 기본 흐름

1. 요청이 Security Filter Chain에 들어옵니다.
2. 인증 정보가 있는지 확인합니다.
3. 인증 객체가 SecurityContext에 저장됩니다.
4. 권한 규칙을 검사합니다.
5. 컨트롤러가 실행됩니다.

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    return http
        .csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/api/admin/**").hasRole("ADMIN")
            .requestMatchers("/api/me").authenticated()
            .anyRequest().permitAll()
        )
        .build();
}
```

CSRF(Cross Site Request Forgery)는 사용자가 의도하지 않은 요청을 보내게 만드는 공격입니다. 세션 기반 웹에서는 중요하고, 토큰 기반 API에서는 구조에 따라 설정을 판단합니다.

## 비밀번호 저장

비밀번호는 평문으로 저장하지 않습니다.

```java
String encoded = passwordEncoder.encode(rawPassword);
```

검증:

```java
boolean matches = passwordEncoder.matches(rawPassword, encodedPassword);
```

## 테스트 종류

- Unit Test: 하나의 클래스나 함수 단위 검증
- Integration Test: DB(Database), Spring Context, 외부 구성과 함께 검증
- E2E(End to End) Test: 사용자 흐름 전체 검증

Spring에서는 목적에 따라 테스트 범위를 조절합니다.

## 단위 테스트

```java
class PasswordPolicyTest {
    @Test
    void passwordMustBeAtLeastEightCharacters() {
        PasswordPolicy policy = new PasswordPolicy();

        assertThatThrownBy(() -> policy.validate("1234"))
            .isInstanceOf(InvalidPasswordException.class);
    }
}
```

도메인 규칙은 Spring 없이 테스트할 수 있어야 합니다.

## 컨트롤러 테스트

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired MockMvc mockMvc;

    @Test
    void createUserValidationError() throws Exception {
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{}"))
            .andExpect(status().isBadRequest());
    }
}
```

HTTP(HyperText Transfer Protocol) 상태 코드, 요청 검증, 응답 형식을 확인합니다.

## 통합 테스트

```java
@SpringBootTest
@Transactional
class SignupServiceIntegrationTest {
    @Autowired SignupService signupService;
    @Autowired UserRepository userRepository;

    @Test
    void signup() {
        Long userId = signupService.signup(new SignupCommand("a@example.com", "password123"));

        assertThat(userRepository.findById(userId)).isPresent();
    }
}
```

실제 DB 동작이 중요한 리포지토리와 트랜잭션은 통합 테스트가 필요합니다.

## 로깅

```java
private static final Logger log = LoggerFactory.getLogger(UserService.class);

log.info("user created. userId={}", userId);
```

로그에 남길 것:

- 요청 식별자
- 사용자 ID
- 핵심 도메인 ID
- 실패 원인
- 외부 API(Application Programming Interface) 응답 시간

로그에 남기면 안 되는 것:

- 비밀번호
- 토큰 전체값
- 주민등록번호 같은 민감 정보
- 결제 카드 전체 번호

## 관측 가능성

관측 가능성은 시스템 밖에서 내부 상태를 추론할 수 있는 능력입니다.

기본 요소:

- Logs: 사건 기록
- Metrics: 숫자로 측정한 상태
- Traces: 요청이 여러 서비스를 지나간 경로

운영 가능한 서버는 "느리다"에서 끝나지 않고 어느 계층이 느린지 좁힐 수 있어야 합니다.

## 체크리스트

- [ ] 인증(Authentication)과 인가(Authorization)를 구분한다.
- [ ] 비밀번호를 평문 저장하지 않는다.
- [ ] 도메인 규칙은 Spring 없이 테스트한다.
- [ ] API 테스트는 상태 코드와 응답 형식을 검증한다.
- [ ] 로그에 민감 정보를 남기지 않는다.
