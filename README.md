[README (3).md](https://github.com/user-attachments/files/25399264/README.3.md)
# 📚 Spring Security & JWT 학습 프로젝트

Spring Boot에서 **Bean 등록 방식부터 Spring Security 기반 로그인까지** 단계적으로 학습한 예제 프로젝트입니다.

---

## 🛠 Tech Stack

- Java 17
- Spring Boot
- Spring Security
- JWT (jjwt)
- JPA / H2
- Lombok
- JUnit 5
- Gradle

---

## 📂 프로젝트 구조

```
spring-auth/
├── config/
│   ├── PasswordConfig.java          # BCryptPasswordEncoder Bean 등록
│   └── WebSecurityConfig.java       # Spring Security 설정 (FilterChain, 로그인 페이지)
├── auth/
│   └── AuthController.java
├── controller/
│   ├── HomeController.java
│   ├── ProductController.java
│   └── UserController.java          # 회원가입 / 로그인 API
├── dto/
│   ├── SignupRequestDto.java
│   └── LoginRequestDto.java
├── entity/
│   ├── User.java                    # 사용자 엔티티 (username, password, email, role)
│   └── UserRoleEnum.java            # ROLE_USER / ROLE_ADMIN
├── filter/
│   ├── LoggingFilter.java           # 로깅 필터
│   └── AuthFilter.java              # JWT 인증 필터 (현재 비활성화)
├── food/
│   ├── Food.java
│   ├── Chicken.java                 # @Primary
│   └── Pizza.java                   # @Qualifier
├── jwt/
│   └── JwtUtil.java                 # JWT 생성 / 검증 / 쿠키 저장
├── repository/
│   └── UserRepository.java
├── security/
│   ├── UserDetailsImpl.java         # UserDetails 구현체
│   └── UserDetailsServiceImpl.java  # UserDetailsService 구현체
├── service/
│   └── UserService.java             # 회원가입 / 로그인 비즈니스 로직
└── test/
    ├── BeanTest.java
    └── PasswordEncoderTest.java
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

### 3️⃣ 회원 기능 구현 (`UserService`)
- 회원가입 API — 중복 username / email 검증 + BCrypt 비밀번호 암호화
- 로그인 API — 사용자 확인 + 비밀번호 검증 후 JWT 발급 → Cookie 저장
- ADMIN 역할 부여 시 Admin Token 검증

### 4️⃣ Filter 구현
- `LoggingFilter` — 모든 요청/응답 로깅
- `AuthFilter` — JWT 쿠키 추출 → 검증 → 사용자 정보 request에 저장
  - `/api/user/**`, `/css/**`, `/js/**` 는 인증 제외

### 5️⃣ Spring Security 적용 (`WebSecurityConfig`)
- `SecurityFilterChain` 설정
- 정적 리소스 접근 허용
- Form 로그인 설정 (로그인 페이지, 처리 URL, 성공/실패 URL)
- `UserDetailsImpl` / `UserDetailsServiceImpl` 구현으로 Spring Security 인증 연동

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

---


## 📖 앞으로 학습 예정 (Roadmap)

### ✅ 완료
- Bean 등록 / DI / `@Primary` / `@Qualifier`
- BCrypt PasswordEncoder
- JWT 생성 / 검증 / 쿠키 저장
- 회원가입 / 로그인 API
- Servlet Filter (LoggingFilter, AuthFilter)
- Spring Security 기본 설정 + Form 로그인
- `UserDetails` / `UserDetailsService` 구현

### 🔜 예정

**Spring Security JWT 로그인**
- `UsernamePasswordAuthenticationFilter` 커스텀
- JWT 발급 필터 구현
- JWT 검증 필터 구현 (요청마다 토큰 검증)

**접근 제어**
- URL별 권한 설정 (`hasRole`, `hasAuthority`)
- ROLE 기반 인가 처리
- 인증 필요 API 보호

**검증 & 예외 처리**
- `@Valid` / `@Validated` 적용
- `@ExceptionHandler` 전역 예외 처리
- 공통 응답 포맷 설계

### 🎯 최종 목표
- JWT 기반 인증 서버 완성
- Access Token + Refresh Token 구조 이해
- 실무 수준의 Spring Security 구조 이해 및 구현
