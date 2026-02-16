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

> Spring 핵심 DI 개념 학습용 프로젝트
