# 📚 Spring Security & JWT 학습 프로젝트

Spring Boot에서 **Bean 등록 방식부터 Spring Security 기반 JWT 로그인까지** 단계적으로 학습한 예제 프로젝트입니다.

---

## 🛠 Tech Stack

- Java 17
- Spring Boot
- Spring Security
- JWT
- JPA / MySQL
- Lombok
- JUnit 5
- Gradle

---

## 📂 프로젝트 구조

```
spring-auth/
├── config/
│   ├── PasswordConfig.java          # BCryptPasswordEncoder Bean 등록
│   └── WebSecurityConfig.java       # Spring Security 설정 (JWT 필터 등록, 접근 제어, STATELESS)
├── auth/
│   └── AuthController.java
├── controller/
│   ├── HomeController.java
│   ├── ProductController.java       # @Secured, @AuthenticationPrincipal, @Valid 사용 예시
│   └── UserController.java          # 회원가입 API
├── dto/
│   ├── SignupRequestDto.java
│   ├── LoginRequestDto.java
│   └── ProductRequestDto.java       # @Valid 검증 어노테이션 적용
├── entity/
│   ├── User.java                    # 사용자 엔티티 (username, password, email, role)
│   └── UserRoleEnum.java            # ROLE_USER / ROLE_ADMIN
├── filter/
│   ├── LoggingFilter.java           # 로깅 필터
│   └── AuthFilter.java              # Servlet 기반 JWT 인증 필터 (현재 비활성화, Spring Security로 대체)
├── food/
│   ├── Food.java
│   ├── Chicken.java                 # @Primary
│   └── Pizza.java                   # @Qualifier
├── jwt/
│   ├── JwtUtil.java                 # JWT 생성 / 검증 / 쿠키 저장
│   ├── JwtAuthenticationFilter.java # 로그인 처리 및 JWT 발급 필터 (UsernamePasswordAuthenticationFilter 확장)
│   └── JwtAuthorizationFilter.java  # 요청마다 JWT 검증 및 SecurityContext 인증 등록 필터
├── repository/
│   └── UserRepository.java
├── security/
│   ├── UserDetailsImpl.java         # UserDetails 구현체
│   └── UserDetailsServiceImpl.java  # UserDetailsService 구현체
├── service/
│   └── UserService.java             # 회원가입 비즈니스 로직 (로그인은 JWT 필터로 처리)
└── test/
    ├── BeanTest.java
    ├── PasswordEncoderTest.java
    └── SpringAuthApplicationTests.java
```

---

## 📌 학습 내용

### 1️⃣ Bean 등록 & DI
- `@Bean`을 이용한 수동 Bean 등록
- 동일 타입 Bean 2개 이상일 때 처리 방법
  - `@Primary` → 기본 선택 Bean (Chicken)
  - `@Qualifier` → 명시적으로 Bean 지정 (Pizza)
- BCrypt PasswordEncoder 등록 및 `matches()`로 비교

### 2️⃣ JWT 구현 (`JwtUtil`)
- JWT 구조 이해 (Header / Payload / Signature)
- HS256 알고리즘으로 Access Token 생성
- `validateToken()`으로 서명 / 만료 검증
- `getUserInfoFromToken()`으로 Claims(사용자 정보) 추출
- 생성된 JWT를 Cookie에 저장 / 꺼내기

### 3️⃣ 회원 기능 구현
- **회원가입** (`UserService`) — 중복 username / email 검증 + BCrypt 비밀번호 암호화
- **로그인** — `JwtAuthenticationFilter`에서 처리 (UserService의 login 메서드는 필터로 대체되어 주석 처리)
- ADMIN 역할 부여 시 Admin Token 검증

### 4️⃣ Filter 구현
- `LoggingFilter` — 모든 요청/응답 로깅 (Servlet Filter)
- `AuthFilter` — JWT 쿠키 추출 → 검증 → 사용자 정보 request에 저장 (현재 `@Component` 비활성화, Spring Security 필터로 대체됨)
  - `/api/user/**`, `/css/**`, `/js/**` 는 인증 제외

### 5️⃣ Spring Security + JWT 필터 적용 (`WebSecurityConfig`)
- Session 방식 비활성화 (`SessionCreationPolicy.STATELESS`)
- CSRF 비활성화
- `JwtAuthenticationFilter` — `UsernamePasswordAuthenticationFilter`를 확장해 `/api/user/login` 처리
  - 로그인 성공 시 JWT 생성 후 쿠키에 저장
  - 로그인 실패 시 401 응답
- `JwtAuthorizationFilter` — `OncePerRequestFilter`를 확장해 모든 요청에서 JWT 검증
  - 토큰이 유효하면 `SecurityContextHolder`에 인증 정보 등록
- 필터 체인 순서: `JwtAuthorizationFilter` → `JwtAuthenticationFilter` → `UsernamePasswordAuthenticationFilter`
- `UserDetailsImpl` / `UserDetailsServiceImpl` 구현으로 Spring Security 인증 연동
- 접근 불가 시 `/forbidden.html` 리다이렉트

### 6️⃣ 접근 제어 & 검증
- `@EnableMethodSecurity(securedEnabled = true)` — 메서드 레벨 보안 활성화
- `@Secured(UserRoleEnum.Authority.ADMIN)` — ADMIN 역할만 접근 가능한 엔드포인트
- `@AuthenticationPrincipal` — SecurityContext에서 현재 로그인 유저 정보 주입
- `@Valid` + Bean Validation 어노테이션 (`@NotBlank`, `@Email`, `@Positive`, `@Size` 등) — 요청 DTO 검증

---

## 🏆 핵심 정리

| 개념 | 설명 |
|---|---|
| `@Primary` | 동일 타입 Bean이 여러 개일 때 기본으로 선택되는 Bean |
| `@Qualifier` | 이름을 명시하여 특정 Bean을 주입 (`@Qualifier` > `@Primary`) |
| JWT | 서버 무상태(Stateless) 인증 토큰, Header/Payload/Signature 구조 |
| BCrypt | 단방향 해시, 비밀번호 저장 시 사용, `matches()`로 비교 |
| `UserDetailsService` | Spring Security가 로그인 시 사용자를 조회하는 인터페이스 |
| `SecurityFilterChain` | Spring Security 요청 처리 흐름 설정 |
| `JwtAuthenticationFilter` | 로그인 요청을 가로채 JWT를 발급하는 필터 |
| `JwtAuthorizationFilter` | 매 요청마다 JWT를 검증하고 인증 객체를 SecurityContext에 등록하는 필터 |
| `SessionCreationPolicy.STATELESS` | 세션을 생성하지 않고 매 요청을 JWT로 인증하는 방식 |
| `@Secured` | 메서드 레벨에서 역할(Role) 기반 접근 제어 |
| `@AuthenticationPrincipal` | SecurityContext에서 현재 인증된 유저 객체를 주입 |
| `@Valid` | 요청 DTO의 Bean Validation 어노테이션을 활성화하여 입력값 검증 |

---

## 📖 학습 현황 (Roadmap)

### ✅ 완료
- Bean 등록 / DI / `@Primary` / `@Qualifier`
- BCrypt PasswordEncoder
- JWT 생성 / 검증 / 쿠키 저장 (`JwtUtil`)
- 회원가입 API
- Servlet Filter (LoggingFilter, AuthFilter)
- Spring Security 기본 설정
- `UserDetails` / `UserDetailsService` 구현
- **`JwtAuthenticationFilter` 구현** — 로그인 처리 및 JWT 발급
- **`JwtAuthorizationFilter` 구현** — 요청마다 JWT 검증 및 인가 처리
- **`SessionCreationPolicy.STATELESS`** — 세션 없는 JWT 인증 구조
- **`@Secured`** — 메서드 레벨 역할 기반 접근 제어
- **`@AuthenticationPrincipal`** — 인증 유저 정보 주입
- **`@Valid` / Bean Validation** — 요청 DTO 입력값 검증

