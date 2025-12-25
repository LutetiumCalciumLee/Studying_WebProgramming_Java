<details>
<summary>ENG (English Version)</summary>

# Spring AOP Functionality

### 1. Cross-Cutting Concerns Problem
Business logic (`memberDAO.updateMember()`) mixed with logging, transactions, security, notifications. AOP separates these **cross-cutting concerns** from core logic, improving maintainability.

### 2. AOP Core Concepts
**Aspect:** Modular cross-cutting concern (logging, security).
**Advice:** Action taken by aspect (before/after method).
**Join Point:** Point where advice can be applied (method execution).
**Pointcut:** Predicate matching join points.
**Target:** Object being advised.
**Weaving:** Process of applying aspects to target.

### 3. AOP Advice Types
**MethodBeforeAdvice:** Executes before target method.
**AfterReturningAdvice:** Executes after successful method completion.
**ThrowsAdvice:** Executes on exceptions.
**MethodInterceptor (Around):** Full control (before, invoke, after).

### 4. ProxyFactoryBean Configuration
```
<bean id="calcTarget" class="Calculator"/>
<bean id="logAdvice" class="LoggingAdvice"/>
<bean id="proxyCal" class="ProxyFactoryBean">
  <property name="target" ref="calcTarget"/>
  <property name="interceptorNames">
    <list><value>logAdvice</value></list>
  </property>
</bean>
```
Creates proxy wrapping `Calculator` with logging.

### 5. MethodInterceptor Implementation
```
public class LoggingAdvice implements MethodInterceptor {
  public Object invoke(MethodInvocation invocation) throws Throwable {
    System.out.println("LoggingAdvice " + invocation.getMethod());
    Object object = invocation.proceed(); // Calls target method
    System.out.println("loggingAdvice " + invocation.getMethod());
    return object;
  }
}
```

### 6. AOP Execution Flow
```
CalcTest.getBean("proxyCal") → LoggingAdvice.invoke() 
→ print "LoggingAdvice add()" → Calculator.add(100,20) 
→ print "120" → print "loggingAdvice add()" → return 120
```

### 7. Project Setup
**lib:** Spring JARs + AOP dependencies.  
**src:** `Calculator.java`, `LoggingAdvice.java`, `CalcTest.java`, `AOPTest.xml`.  
**Test:** `ApplicationContext context = new ClassPathXmlApplicationContext("AOPTest.xml");`

### 8. Dynamic Proxy Creation
Spring creates JDK dynamic proxy (interface-based) or CGLIB proxy (class-based). Transparent to application code - `proxyCal.add()` automatically triggers advice.

### 9. Benefits Demonstrated
**Before AOP:** Logging scattered across methods.  
**After AOP:** Single `LoggingAdvice` applies to all calculator methods. **DRY principle** achieved.

### 10. Common Use Cases
- **Logging:** Method entry/exit timestamps.
- **Transactions:** `@Transactional` (auto-commit/rollback).
- **Security:** Authority checks before operations.
- **Performance:** Method timing/profiling.

### 11. XML vs Annotation AOP
**XML (Spring 2.0):** `<aop:config>` + `<aop:aspect>`.  
**Annotation (Spring 3.0+):** `@Aspect`, `@Before`, `@After`, `@Around`.

### 12. Pointcut Expressions
**execution:** `execution(* com.spring..*.*(..))` matches all methods.  
**args:** Matches by argument types.  
**within:** Matches by class/package.

### 13. Weaving Types
**Compile-time:** AspectJ compiler.  
**Load-time:** Bytecode modification at class load.  
**Runtime:** Dynamic proxies (Spring default).

### 14. Testing AOP
```
Calculator cal = (Calculator) context.getBean("proxyCal");
cal.add(100, 20); // Triggers LoggingAdvice automatically
```

### 15. Evolution to Modern Spring
**Spring 2.x:** ProxyFactoryBean + XML.  
**Spring 3.x+:** `@EnableAspectJAutoProxy` + `@Aspect`.  
**Spring 5.x+:** Reactive AOP support.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 AOP 기능

### 1. 횡단 관심사 문제
비즈니스 로직(`memberDAO.updateMember()`)에 로깅, 트랜잭션, 보안, 알림 혼재. AOP는 이러한 **횡단 관심사**를 핵심 로직에서 분리하여 유지보수성 향상.

### 2. AOP 핵심 개념
**Aspect:** 모듈화된 횡단 관심사(로깅, 보안).
**Advice:** Aspect가 수행하는 동작(메서드 전/후).
**Join Point:** Advice 적용 가능한 지점(메서드 실행).
**Pointcut:** Join Point 매칭 조건.
**Target:** 조언 대상 객체.
**Weaving:** Aspect 적용 과정.

### 3. AOP Advice 유형
**MethodBeforeAdvice:** 타겟 메서드 전 실행.
**AfterReturningAdvice:** 성공적 메서드 완료 후 실행.
**ThrowsAdvice:** 예외 발생 시 실행.
**MethodInterceptor (Around):** 완전 제어(전, 호출, 후).

### 4. ProxyFactoryBean 설정
```
<bean id="calcTarget" class="Calculator"/>
<bean id="logAdvice" class="LoggingAdvice"/>
<bean id="proxyCal" class="ProxyFactoryBean">
  <property name="target" ref="calcTarget"/>
  <property name="interceptorNames">
    <list><value>logAdvice</value></list>
  </property>
</bean>
```
`Calculator`를 로깅으로 감싼 프록시 생성.

### 5. MethodInterceptor 구현
```
public class LoggingAdvice implements MethodInterceptor {
  public Object invoke(MethodInvocation invocation) throws Throwable {
    System.out.println("LoggingAdvice " + invocation.getMethod());
    Object object = invocation.proceed(); // 타겟 메서드 호출
    System.out.println("loggingAdvice " + invocation.getMethod());
    return object;
  }
}
```

### 6. AOP 실행 흐름
```
CalcTest.getBean("proxyCal") → LoggingAdvice.invoke() 
→ "LoggingAdvice add()" 출력 → Calculator.add(100,20) 
→ "120" 출력 → "loggingAdvice add()" → 120 반환
```

### 7. 프로젝트 설정
**lib:** Spring JAR + AOP 의존성.  
**src:** `Calculator.java`, `LoggingAdvice.java`, `CalcTest.java`, `AOPTest.xml`.  
**테스트:** `ClassPathXmlApplicationContext("AOPTest.xml")`.

### 8. 동적 프록시 생성
Spring이 JDK 동적 프록시(인터페이스 기반) 또는 CGLIB 프록시(클래스 기반) 생성. 애플리케이션 코드에 투명 - `proxyCal.add()`가 자동으로 Advice 트리거.

### 9. 입증된 장점
**AOP 전:** 로깅이 메서드마다 분산.  
**AOP 후:** 단일 `LoggingAdvice`가 모든 계산기 메서드에 적용. **DRY 원칙** 달성.

### 10. 일반 사용 사례
- **로깅:** 메서드 진입/종료 타임스탬프.
- **트랜잭션:** `@Transactional` (자동 커밋/롤백).
- **보안:** 작업 전 권한 체크.
- **성능:** 메서드 실행 시간 측정.

### 11. XML vs 어노테이션 AOP
**XML (Spring 2.0):** `<aop:config>` + `<aop:aspect>`.  
**어노테이션 (Spring 3.0+):** `@Aspect`, `@Before`, `@After`, `@Around`.

### 12. Pointcut 표현식
**execution:** `execution(* com.spring..*.*(..))` 모든 메서드 매칭.  
**args:** 인자 타입으로 매칭.  
**within:** 클래스/패키지로 매칭.

### 13. Weaving 유형
**컴파일 타임:** AspectJ 컴파일러.  
**로드 타임:** 클래스 로드 시 바이트코드 수정.  
**런타임:** 동적 프록시 (Spring 기본).

### 14. AOP 테스트
```
Calculator cal = (Calculator) context.getBean("proxyCal");
cal.add(100, 20); // LoggingAdvice 자동 트리거
```

### 15. 현대 Spring으로의 진화
**Spring 2.x:** ProxyFactoryBean + XML.  
**Spring 3.x+:** `@EnableAspectJAutoProxy` + `@Aspect`.  
**Spring 5.x+:** 반응형 AOP 지원.

</details>
