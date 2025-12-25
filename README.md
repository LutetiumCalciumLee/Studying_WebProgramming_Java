<details>
<summary>ENG (English Version)</summary>

# Spring Dependency Injection & IoC

### 1. Tight vs Loose Coupling Problem
Traditional code creates dependencies directly (`boardService = new BoardService()`), causing tight coupling. Changing DAO (Oracle→MySQL) requires modifying all dependent classes. Spring solves via IoC/DI.

### 2. Interface-Based Loose Coupling
**Before:** `BoardController → BoardService → BoardDAO` (concrete classes). **After:** Interfaces (`BoardService`, `BoardDAO`) with multiple implementations (`BoardOracleDAOImpl`, `BoardMySqlDAOImpl`). Switch implementations without code changes.

### 3. IoC & DI Core Concepts
**IoC (Inversion of Control):** Framework controls object creation/lifecycle. **DI (Dependency Injection):** Dependencies injected via constructor or setter. Eliminates `new` keyword in business logic.

### 4. Setter Injection Pattern
```
<bean id="personService" class="PersonServiceImpl">
  <property name="name" value="John"/>
</bean>
```
Java: `public void setName(String name) { this.name = name; }`

### 5. Constructor Injection Pattern
```
<bean id="personService1" class="PersonServiceImpl">
  <constructor-arg value="John"/>
</bean>
<bean id="personService2" class="PersonServiceImpl">
  <constructor-arg value="John"/>
  <constructor-arg value="23"/>
</bean>
```

### 6. Reference Injection (Complex Dependencies)
```
<bean id="memberService" class="MemberServiceImpl">
  <property name="memberDAO" ref="memberDAO"/>
</bean>
<bean id="memberDAO" class="MemberDAOImpl"/>
```
`MemberServiceImpl.setMemberDAO()` receives `MemberDAOImpl` instance.

### 7. Spring Container Setup
**XML:** `BeanFactory factory = new XmlBeanFactory(new FileSystemResource("person.xml"));`
**Modern:** `ApplicationContext context = new FileSystemXmlApplicationContext("person.xml");`
**Bean Retrieval:** `PersonService person = (PersonService) factory.getBean("personService");`

### 8. Lazy Initialization (`lazy-init`)
- `lazy-init="false"` (default): Beans created on container startup.
- `lazy-init="true"`: Beans created on first `getBean()` call.
- Test: `First`/`Third` instantiate immediately, `Second` only when accessed.

### 9. Bean Lifecycle
1. **XML Loading:** Container reads bean definitions.
2. **Instantiation:** Creates objects (unless lazy-init).
3. **Dependency Resolution:** Injects setter/constructor dependencies.
4. **Initialization:** Calls `init-method` (if defined).
5. **Usage:** Application retrieves via `getBean()`.

### 10. Project Structure Example
```
pro19/
├── src/
│   ├── com.spring.ex01/ (PersonService interface/impl)
│   ├── com.spring.ex03/ (MemberService → MemberDAO injection)
│   └── person.xml, member.xml
├── lib/ (spring.jar, commons-*.jar)
└── Referenced Libraries
```

### 11. Testing DI
**Manual:** `PersonService person = new PersonServiceImpl();` (tight coupling).  
**Spring:** `PersonService person = factory.getBean("personService");` (dependencies auto-wired).

### 12. Benefits Demonstrated
- **Switch DB:** Change `<bean id="memberDAO" class="BoardMySqlDAOImpl"/>` → no code changes.
- **Configuration:** Externalize via XML (no recompilation).
- **Testability:** Mock dependencies easily via DI.

### 13. Common XML Attributes
- `id`: Bean identifier.
- `class`: Fully qualified class name.
- `property`: Setter injection.
- `constructor-arg`: Constructor injection.
- `ref`: Reference other beans.
- `lazy-init`: Lazy loading control.

### 14. Evolution Path
**Spring 2.0:** XML-heavy (shown in examples).  
**Spring 3.0+:** Annotations (`@Component`, `@Autowired`).  
**Spring 4.0+:** JavaConfig (`@Configuration`, `@Bean`).  
**Spring 5.0+:** Reactive programming support.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 의존성 주입과 제어 역전

### 1. Tight vs Loose Coupling 문제
전통 코드에서 직접 의존성 생성(`boardService = new BoardService()`) → tight coupling. DAO 변경(Oracle→MySQL) 시 모든 클래스 수정 필요. 스프링은 IoC/DI로 해결.

### 2. 인터페이스 기반 Loose Coupling
**이전:** `BoardController → BoardService → BoardDAO` (구체 클래스). **이후:** 인터페이스(`BoardService`, `BoardDAO`) + 다중 구현체(`BoardOracleDAOImpl`, `BoardMySqlDAOImpl`). 코드 변경 없이 구현체 전환.

### 3. IoC & DI 핵심 개념
**IoC (제어의 역전):** 프레임워크가 객체 생성/수명 주기 제어. **DI (의존성 주입):** 생성자/Setter로 의존성 주입. 비즈니스 로직에서 `new` 키워드 제거.

### 4. Setter 주입 패턴
```
<bean id="personService" class="PersonServiceImpl">
  <property name="name" value="John"/>
</bean>
```
Java: `public void setName(String name) { this.name = name; }`

### 5. 생성자 주입 패턴
```
<bean id="personService1" class="PersonServiceImpl">
  <constructor-arg value="John"/>
</bean>
<bean id="personService2" class="PersonServiceImpl">
  <constructor-arg value="John"/>
  <constructor-arg value="23"/>
</bean>
```

### 6. 참조 주입 (복잡한 의존성)
```
<bean id="memberService" class="MemberServiceImpl">
  <property name="memberDAO" ref="memberDAO"/>
</bean>
<bean id="memberDAO" class="MemberDAOImpl"/>
```
`MemberServiceImpl.setMemberDAO()`가 `MemberDAOImpl` 인스턴스 받음.

### 7. 스프링 컨테이너 설정
**XML:** `BeanFactory factory = new XmlBeanFactory(new FileSystemResource("person.xml"));`
**현대:** `ApplicationContext context = new FileSystemXmlApplicationContext("person.xml");`
**Bean 조회:** `PersonService person = (PersonService) factory.getBean("personService");`

### 8. 지연 초기화 (`lazy-init`)
- `lazy-init="false"` (기본): 컨테이너 시작 시 Bean 생성.
- `lazy-init="true"`: 첫 `getBean()` 호출 시 생성.
- 테스트: `First`/`Third` 즉시 생성, `Second` 접근 시 생성.

### 9. Bean 생명주기
1. **XML 로딩:** Bean 정의 읽기.
2. **인스턴스화:** 객체 생성 (lazy-init 제외).
3. **의존성 해소:** Setter/생성자 주입.
4. **초기화:** `init-method` 호출 (정의 시).
5. **사용:** `getBean()`으로 애플리케이션 조회.

### 10. 프로젝트 구조 예시
```
pro19/
├── src/
│   ├── com.spring.ex01/ (PersonService 인터페이스/구현)
│   ├── com.spring.ex03/ (MemberService → MemberDAO 주입)
│   └── person.xml, member.xml
├── lib/ (spring.jar, commons-*.jar)
└── Referenced Libraries
```

### 11. DI 테스트
**수동:** `PersonService person = new PersonServiceImpl();` (tight coupling).  
**Spring:** `PersonService person = factory.getBean("personService");` (의존성 자동 연결).

### 12. 입증된 장점
- **DB 전환:** `<bean id="memberDAO" class="BoardMySqlDAOImpl"/>` 변경 → 코드 변경 없음.
- **설정:** XML로 외부화 (재컴파일 불필요).
- **테스트성:** DI로 Mock 의존성 주입 용이.

### 13. 일반 XML 속성
- `id`: Bean 식별자.
- `class`: 완전한 클래스명.
- `property`: Setter 주입.
- `constructor-arg`: 생성자 주입.
- `ref`: 다른 Bean 참조.
- `lazy-init`: 지연 로딩 제어.

### 14. 진화 경로
**Spring 2.0:** XML 중심 (예제와 동일).  
**Spring 3.0+:** 어노테이션 (`@Component`, `@Autowired`).  
**Spring 4.0+:** JavaConfig (`@Configuration`, `@Bean`).  
**Spring 5.0+:** 반응형 프로그래밍 지원.

</details>
