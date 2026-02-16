# 📚 Spring Bean & DI 학습 프로젝트

Spring Boot에서 **Bean 등록 방식과 동일 타입 Bean 충돌 해결 방법**을 학습한 예제 프로젝트입니다.

---

## 🛠 Tech Stack

- Java 17
- Spring Boot
- Spring Security (PasswordEncoder)
- JUnit5
- Gradle

---

## 📌 학습 내용

- `@Bean`을 이용한 수동 Bean 등록
- 동일 타입 Bean 2개 이상일 때 처리 방법
- `@Primary`
- `@Qualifier`
- BCrypt PasswordEncoder 사용

---

## 📂 프로젝트 구조

```
spring-auth/
 ├── config/
 │     └── PasswordConfig.java
 ├── food/
 │     ├── Food.java
 │     ├── Chicken.java (@Primary)
 │     └── Pizza.java (@Qualifier)
 ├── BeanTest.java
 └── PasswordEncoderTest.java
```

---

## 🏆 핵심 정리

- 동일 타입 Bean이 여러 개면 충돌 발생
- `@Qualifier` → 가장 우선
- `@Primary` → 기본 선택 Bean
- BCrypt는 `matches()`로 비교

---

## 🚀 실행 방법

```
./gradlew build
./gradlew test
```

---

---

# 📖 앞으로 학습 예정 (Roadmap)

## 1️⃣ 인증 / 인가 기초

- 인증(Authentication) vs 인가(Authorization) 개념 이해
- Spring Security 동작 원리
- Security Filter 흐름 이해

---

## 2️⃣ JWT 구현

- JWT 구조 이해 (Header / Payload / Signature)
- JWT 생성
- JWT 검증
- Access Token 발급
- 로그인 시 토큰 반환

---

## 3️⃣ 회원 기능 구현

- 회원가입 API
- 비밀번호 암호화 적용
- 로그인 API
- JWT 기반 인증 처리

---

## 4️⃣ Spring Security 적용

- Security 설정 클래스 작성
- 로그인 필터 구현
- JWT 필터 구현
- 인증 성공 / 실패 처리

---

## 5️⃣ 접근 제어

- URL 권한 설정
- Role 기반 인가 처리
- 인증 필요 API 보호

---

## 6️⃣ 검증 & 예외 처리

- Validation 적용
- 예외 처리 핸들러 구현
- 공통 응답 포맷 설계

---

## 🎯 최종 목표

- JWT 기반 인증 서버 완성
- 로그인 / 회원가입 기능 구현
- 인증이 필요한 API 보호
- 실무 수준의 Security 구조 이해

> Spring 핵심 DI 개념 학습용 프로젝트
