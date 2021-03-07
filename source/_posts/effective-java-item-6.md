---
title: "[EFFECTIVE JAVA] Item 6 불필요헌 객체 생성을 피하라"
catalog: true
date: 2021-03-08
subtitle: ""
header-img:
tags:
- Effactive Java
categories:
- 개발
- Java
---
> ```java 
> String s = new String("bikini"); // 따라 하지 말 것 !
> ```
- 이 문장은 실행 될 때 마다 String 인스턴스를 새로 만든다.
- 빈번히 호출되는 메서드 안에 있다면 쓸 때 없는 인스턴스를 계속 만들게 된다.

```java 
// 개선된 버전
String s = "bikini";
```
- 같은 가상 머신 안에서 똑같은 문자열 리터럴을 사용하는 모든 코드가 같은 객체를 사용한다는걸 보장한다.

## 불필요한 객체 생성을 피하는 방법
- 불변 클래스에서는 정적 팩터리 메서드를 사용해 불필요한 객체생성을 피할 수 있다.
    - `new Boolean(String)` 대신 `Boolean.valueOf(String)`
    - 생성자는 호출할 때마다 새로운 객체를 생성하지만, 팩터리 메서드는 그렇지 않다.
- 가변 객체라 해도 사용 중에 변경되지 않는 다면 재사용할 수 있다.
- '비싼 객체'가 반복해서 필요하게 되면 캐싱하여 재사용하길 권한다.
```java
public class RomanNumerals {

    // Pattern 인스턴스를 클래스 초기화 과정에서 직접 생성해 캐싱해두고 다른 메서드에서 이 인스턴스를 재사용한다.
    private static final Pattern ROMAN = Pattern.compile(...);

    static boolean isRomanNumeral(String s) {
        return ROMAN.matcher(s).matches();
    }
}
```
- 지연 초기화로 불필요한 초기화를 없앨 수는 있지만 코드를 복잡하게 만들기만 하고 성능이 개선되지 않을 경우가 많다.
    - 서버가 켜지면서 초기화 하는 것과 런타임 중에 초기화 되는 경우를 잘 생각해보자.

## 불필요한 객체를 만들어 내는 경우
- 오토 방싱(auto boxing)
    - 오토 박싱은 기본 타입과 그에 대응하는 박싱된 기본 타입의 구분을 흐려주지만, 완전히 없애주는건 아니다.
    - 오토 박싱을 하면서 객체를 생성하게 된다.
    - 박싱된 기본 타입보다는 기본 타입을 사용하고, 의도치 않은 오토박싱이 숨어들지 않도록 주의하자.
```java
private static long sum() {
    Long sum = 0L;
    for (long i = 0; i <= Integer.MAX_VALUE; i++)
        sum += i; // 오토박싱이 이뤄지는 곳.. 오토박싱으로 객체를 계속 생성한다.

    return sum;
}
```