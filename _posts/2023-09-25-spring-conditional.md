---
title: @Conditional
date: 2023-09-25 18:00:00 +0800
categories: [spring]
tags: [spring]
---

## @Conditional

```java
@Configuration
public class MyConfiguration {

    @Bean
    @Conditional(MyCondition.class) // MyCondition 조건에 따라 활성화
    public MyBean myBean() {
        return new MyBean();
    }
}
```

## ConditionalOnXXX

### @ConditionalOnClass

```java
@Configuration
@ConditionalOnClass({TestClass.class})
public class MyConfiguration {}
```

TestClass 클래스가 클래스 패스에 존재할 때만 자동 구성 또는 빈을 활성화한다.

### @ConditionalOnProperty

```java
@Configuration
@ConditionalOnProperty(name = "myapp.feature.enabled", havingValue = "true")
public class MyConfiguration {}
```

myapp.feature.enabled 프로퍼티가 true일 때만 활성화한다.

### @ConditionalOnBean

```java
@Configuration
@ConditionalOnBean(MyService.class)
public class MyConfiguration {

}
```

MyService 타입의 빈이 컨텍스트에 있을 때만 활성화됨

### 그외

ConditionalOnCloudPlatform
ConditionalOnExpression
ConditionalOnJava
ConditionalOnJndi
ConditionalOnNotWarDeployment
ConditionalOnNotWebApplication
ConditionalOnResource
ConditionalOnSingleCandidate
ConditionalOnWarDeployment
ConditionalOnWebApplication

---

## @ConditionalOnMissingXXX

### @ConditionalOnMissingClass

```java
@Configuration
@ConditionalOnMissingClass("com.example.TestClass")
public class MyConfiguration {}
```

TestClass 클래스가 클래스 패스에 존재하지 않을 때만 자동 구성 또는 빈을 활성화한다.


### @ConditionalOnMissingBean

```java
@Configuration
@ConditionalOnMissingBean(MyService.class)
public class MyConfiguration {

}
```

MyService 타입의 빈이 컨텍스트에 없을 때만 활성화됨
