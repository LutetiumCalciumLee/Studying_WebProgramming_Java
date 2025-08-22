# Chapter 20: Spring AOP Features — Summary (English)

## 1. Emergence of Aspect-Oriented Programming (AOP)

- Problems with the traditional approach:
  - Every time implementing a core feature (e.g., member level upgrade), developers embed auxiliary features such as logging, security, and transactions directly inside methods.
  - As the application grows, duplicated code increases, source code becomes complex, and maintenance becomes difficult.

- Solution idea:
  - Use AOP to separate core features from cross-cutting concerns (logging/security/transactions) and apply them declaratively at required join points.
  - As a result, cohesion increases, duplication decreases, and maintainability improves.

## 2. Using AOP in Spring

### Key terminology (concepts)
- Target: The actual business object (class/method) to which auxiliary features are applied.
- Advice: The auxiliary logic to apply (e.g., logging, transaction).
- Pointcut: The expression that designates the exact methods/points where advice is applied.
- Advisor: A combination of pointcut and advice.
- Proxy: A runtime wrapper around the target that weaves and executes advice around method calls.

### Spring API-based AOP implementation flow
1) Specify the target class  
2) Implement the advice class  
3) Define the pointcut in the configuration  
4) Combine pointcut and advice with an advisor  
5) Use ProxyFactoryBean to connect the target and advice (create a proxy bean)  
6) Obtain and use the proxy bean via getBean()

### Practice overview (pro20)
- Configuration file: AOPTest.xml
- Main classes:
  - Calculator: Target (methods for add, subtract, multiply, divide)
  - LoggingAdvice: Implements MethodInterceptor, performs logging before/after method execution
  - CalcTest: Loads the context and invokes methods via the proxy bean

### Configuration and code highlights
- AOPTest.xml
  - calcTarget: com.spring.ex01.Calculator (target bean)
  - logAdvice: com.spring.ex01.LoggingAdvice (advice bean)
  - proxyCal: org.springframework.aop.framework.ProxyFactoryBean
    - target = calcTarget
    - interceptorNames includes logAdvice
- LoggingAdvice (invoke):
  - Print logs before the method call
  - Execute the actual target method with invocation.proceed()
  - Print logs after the method call
- Execution result:
  - When calling add/subtract/multiply/divide, logging messages are printed before and after each method, and the actual operation results appear
  - Cross-cutting logic is applied consistently without modifying the target code
