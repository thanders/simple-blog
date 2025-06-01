# Aspect Oriented Programming (AOP) with Spring Boot

## What is AOP?

Aspect-Oriented Programming (AOP) is a programming paradigm that provides a way to modularize cross-cutting concerns.

It achieves this by defining:

- An Aspect: A module that encapsulates a cross-cutting concern (e.g., logging, security checking).
- Join Points: Specific execution points in your program (like method calls, exception throwing).
- Advice: The actual code or action that an Aspect takes at a Join Point (e.g., "log this message before the method runs").
- Pointcuts: Expressions that select specific Join Points where the Advice should be applied.
- Weaving: The process of linking Aspects with your regular application code to create the final, "advised" application.

## Why is this useful?

This is useful because it allows you to define cross cutting concerns (Aspects), apply them to specific methods or exceptions (Join Points) and execute specific code (Advice).

You can even define expressions (Pointcuts) which select specific join points to execute pointcuts.

## How does weaving work?

In Spring AOP, proxies are the fundamental mechanism for runtime weaving.

Instead of changing your original code, Spring creates a proxy object that wraps your actual bean. When a method is called, the proxy intercepts the call, executes the AOP advice (e.g., logging, security), and then passes the call to the original method.

## What are the different types of AOP advices?

**@Before Advice**

Runs before the main method executes.

**@AfterReturning Advice**

Runs after the main method finishes successfully.

**@AfterThrowing Advice**

Runs after the main method throws an exception.

**@After (Finally) Advice**

Runs after the main method, always (whether it succeeds or fails).

**@Around Advice**

Runs around the main method, giving full control to execute, modify, or skip it.

## Spring Boot uses AOP for Annotations

Spring Boot uses AOP for following annotations.

**@Transactional**: Manages database transactions automatically (commit on success, rollback on error).

**@Cacheable / @CachePut / @CacheEvict**: Handles method caching to improve performance by storing and retrieving results.

**@Async**: Executes methods in a separate thread, allowing non-blocking calls.

**@Retryable**: Automatically retries method execution if it fails (from Spring Retry).

**@Timed**: Captures method execution time as metrics (often used with Actuator/Micrometer).

**@Secured / @PreAuthorize / @PostAuthorize**: Enforces security rules and permissions on methods (from Spring Security).

## Simple Example

```java
// src/main/java/com/example/simpleblog/aspects/LoggingAspect.java
package com.example.simpleblog.aspects; // UPDATED PACKAGE TO INCLUDE 'aspects'

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.simpleblog.MyService.performAction(..))")
    public void logBeforePerformAction(JoinPoint joinPoint) {
        System.out.println("--- Aspect: Logging before method: " + joinPoint.getSignature().getName() + " ---");
    }
}
```

### Summary

Advice - @Before

Pointcut - `"execution(* com.example.simpleblog.MyService.performAction(..))"`

An AspectJ expression which will match:

- `*` any return type

- `(..)`: Any number (zero or more) of arguments of any type.

- Any call to the `performAction` method within `com.example.simpleblog.MyService`