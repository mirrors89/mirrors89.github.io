---
title: "[EFFECTIVE JAVA] Item 5 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라"
catalog: true
date: 2021-03-07
subtitle: ""
header-img:
tags:
- Effactive Java
categories:
- 개발
- Java
---
## 하나 이상의 자원을 의존하여 사용하는 경우 유틸리티 클래스나 싱글턴 패턴을 사용한 경우
```java
// 정적 유틸리티를 잘못 사용한 예 - 유연하지 않고 테스트하기 어렵다.
public class SpellChecker {
    private final Lexicon dictionary = ...;

    private SpellChecker() {}

    public static boolean isValid(String word) { ... }
    public static List<String> suggestions(String typo) { ... }
}
```

```java
// 싱글턴을 잘못 사용한 예 - 유연하지 않고 테스트하기 어렵다.
public class SpellChecker {
    private final Lexicon dictionary = ...;

    private SpellChecker(...) {}
    public static SpecllChecker INSTANCE = new SpellChecker(...);

    public static boolean isValid(String word) { ... }
    public static List<String> suggestions(String typo) { ... }
}
```
- 테스트용도로 사용할 자원이나 다른 용도의 자원으로 교체하기 어렵다.
- final 한정자를 제거하고 다른 자원으로 교체하는 메서드를 만들 수 있지만 오류를 내기 쉬우며 멀티스레드 환경에선 사용할 수 없다. (Thread Safe 하지 않다.)
- 사용하는 자원에 따라 동작이 달라지는 클래스에서는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.

## 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식 (Dependency Injection)
```java
// 의존 객체 주입은 유연성과 테스트 용이성을 높여준다.
public class SpellChecker {
    private final Lexicon dictionary;

    private SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public static boolean isValid(String word) { ... }
    public static List<String> suggestions(String typo) { ... }
}
```
- 자원이 몇개든 의존 관계가 어떻든 상관없이 잘 동작한다.
- 불변을 보장하여 여러 클라이언트가 의존 객체를 안심하고 공유할 수 있다.
- 의존 객체 주입은 생성자, 정적 팩터리, 빌더에 똑같이 응용할 수 있다.