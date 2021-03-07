---
title: "[EFFECTIVE JAVA] Item 4 인스턴스화를 막으려거든 private 생성자를 사용하라"
catalog: true
date: 2021-02-03
subtitle: ""
header-img:
tags:
- Effactive Java
categories:
- 개발
- Java
---
## 정적 메서드와 정적 필드만 담은 클래스를 만들고 싶은 경우
- 상수나 유틸성 메소드를 모으로 싶은 경우
- 특정 인터페이스를 구현하는 객체를 생성해주는 메서드를 모으고 싶은 경우
- final 클래스와 관련한 메서드 들을 모아놓을 때
> 남용하게 되면 객체 지향적으로 사고하지 않는 방식이므로 위 3가지 이유가 아니라면 사용하지 않는 방향으로 고려해보자


## 추상 클래스로 만드는 것은 인스턴스화를 막을 수 없다.
- 생성자를 명시하지 않으면 컴파일러가 자동으로 기본 생성자를 만들어준다.
- private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있다.
```java
public class PrivateConstructorClass {
    private PrivateConstructorClass() {
        throw new AssertionError();
    }
}
```
- 클래스 안에서 실수라도 생성자를 호출하지 않도록 해준다.
- 어떤 환경에서도 클래스가 인스턴스화 되는 것을 막아준다.
- 상속을 불가능 하게 하는 효과도 있다.
    - 모든 생성자는 상위 클래스의 생성자를 호출 하게되는데, private로 선언하면 하위 클래스가 상위 클래스의 생성자의 접근을 막아버린다.
