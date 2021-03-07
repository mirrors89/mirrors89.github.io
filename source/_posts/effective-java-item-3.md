---
title: "[EFFECTIVE JAVA] Item 3 private 생성자나 열거 타입으로 싱글턴임을 보증하라"
catalog: true
date: 2021-01-31
subtitle: ""
header-img:
tags:
- Effactive Java
categories:
- 개발
- Java
---

## 싱글턴이란?
- 인스턴스를 오직 하나만 생성할 수 있는 클래스
- 함수와 같은 무상태(stateless) 객체나 설계상 유일해야 하는 시스템 컴포넌트 등

## 클래스를 싱글턴으로 만들면 테스트하기 어려워질 수 있다.
- 싱글턴을 mock 구현으로 대체할 수 없기 때문

## 싱글턴을 만드는 방식

### public static 멤버가 final 필드인 방식
```java
public class SingletonClass {
    public static final SingletonClass INSTANCE = new SingletonClass();
    private SingletonClass() {}
}
```
- private 생성자는 `SingletonClass.INSTANCE` 초기화 할 때 딱 한 번만 호출된다. 
- public이나 protected 생성자가 없으므로 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나임이 보장된다
- 다만, 리플렉션 API인 `AccessibleObject.setAccessible`을 사용해 생성자를 호출할 수 있다.
    - 생성자를 수정해서 두번째 객체가 생성하려 때 예외를 던지도록 해 방어할 수 있다.

##### 장점
- 해당 클래스가 싱글턴임이 API에 드러난다.
- 간결함

### 정적 팩터리 메서드를 public static 멤버로 제공하는 방식
```java
public class SingletonClass {
    private static final SingletonClass INSTANCE = new SingletonClass();
    private SingletonClass() {}

    public static SingletonClass getInstance() {
        return INSTANCE;
    }
}
```
##### 장점
- API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다.
- 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다.
- 정적 팩터리의 메서드를 공급자(supplier)로 사용할 수 있다.
    - 예) `Supplier<SingletonClass>`


> 위 두가지 방식으로 만든 싱글턴 클래스를 직렬화하려면 readResolve 메서드를 제공해야한다.
> 이렇게 하지 않으면 역직렬할 때 마다 새로운 인스턴스가 만들어진다.
> ```java
> private Object readResolve() {
>     return INSTANCE;    
> }
> ```


### 열거 타입으로 선언하는 방식
```java
public enum SingletonEnum {
    INSTANCE
}
```
- public 필드 방식과 비슷하지만, 더 간결하고, 추가 노력없이 직렬화할 수 있다.
- 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽히 막아준다.
- 대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.