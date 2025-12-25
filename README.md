<details>
<summary>ENG (English Version)</summary>

# Spring Framework Introduction

### 1. Framework Evolution
Traditional EJB (Enterprise JavaBeans) was heavyweight with complex container requirements. Spring emerged as lightweight framework supporting POJOs (Plain Old Java Objects) with simplified configuration.

### 2. Core Spring Concepts
**IoC (Inversion of Control):** Framework manages object lifecycle and dependencies (reverses control from developers).
**DI (Dependency Injection):** Objects receive dependencies via constructor/setter rather than creating them.
**AOP (Aspect-Oriented Programming):** Cross-cutting concerns (logging, transactions) separated from business logic.

### 3. Spring Architecture Modules
**Core Container:** IoC, Bean management, Context.
**AOP:** Aspect-oriented features.
**DAO/ORM:** JDBC abstraction, Hibernate/MyBatis integration.
**Web/MVC:** Spring MVC for web applications.
**Additional:** Security, Test, Batch modules.

### 4. Spring vs Traditional Java EE
**Traditional:** Heavyweight EJB, XML-heavy config, invasive APIs.
**Spring:** POJO-based, annotation-driven, lightweight container, extensive auto-configuration.

### 5. Project Setup Requirements
Spring 3.0 requires multiple JARs in WEB-INF/lib: `spring-core`, `spring-context`, `spring-beans`, `spring-aop`, `spring-web`, `spring-webmvc`, plus dependencies like `commons-logging`, `cglib`, `aopalliance`.

### 6. Key Benefits
- **Loose Coupling:** DI reduces direct dependencies.
- **Testability:** POJOs easily unit testable.
- **Modularity:** Mix/match modules as needed.
- **Configuration:** XML/annotation-based, hot-reloadable.

### 7. Basic Configuration Pattern
```
<context:component-scan base-package="com.example"/>
<mvc:annotation-driven/>
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"/>
```
Enables component scanning, MVC, and database connection pooling.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 프레임워크 시작하기

### 1. 프레임워크 진화
전통적인 EJB(Enterprise JavaBeans)는 복잡한 컨테이너 요구사항으로 무거움. 스프링은 POJO(Plain Old Java Object)를 지원하는 경량 프레임워크로 등장.

### 2. 스프링 핵심 개념
**IoC (제어의 역전):** 프레임워크가 객체 생명주기와 의존성 관리.
**DI (의존성 주입):** 생성자/Setter로 의존성 주입.
**AOP (관점 지향 프로그래밍):** 로깅, 트랜잭션 등 횡단 관심사 분리.

### 3. 스프링 아키텍처 모듈
**Core Container:** IoC, Bean 관리, Context.
**AOP:** 관점 지향 기능.
**DAO/ORM:** JDBC 추상화, Hibernate/MyBatis 통합.
**Web/MVC:** Spring MVC 웹 애플리케이션.
**추가:** Security, Test, Batch 모듈.

### 4. 스프링 vs 전통 Java EE
**전통:** 무거운 EJB, XML 과다, 침투적 API.
**스프링:** POJO 기반, 어노테이션 주도, 경량 컨테이너, 광범위 자동 설정.

### 5. 프로젝트 설정 요구사항
Spring 3.0은 WEB-INF/lib에 다수 JAR 필요: `spring-core`, `spring-context`, `spring-beans`, `spring-aop`, `spring-web`, `spring-webmvc` + `commons-logging`, `cglib`, `aopalliance` 등 의존성.

### 6. 주요 장점
- **느슨한 결합:** DI로 직접 의존성 감소.
- **테스트 용이성:** POJO 단위 테스트 간편.
- **모듈성:** 필요한 모듈만 선택.
- **설정:** XML/어노테이션 기반, 핫 리로드 가능.

### 7. 기본 설정 패턴
```
<context:component-scan base-package="com.example"/>
<mvc:annotation-driven/>
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"/>
```
컴포넌트 스캔, MVC, 데이터베이스 커넥션 풀 활성화.

</details>
