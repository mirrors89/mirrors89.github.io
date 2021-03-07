---
title: "[EFFECTIVE JAVA] Item 2 생성자에 매개변수가 많다면 빌더를 고려하라"
catalog: true
date: 2021-01-28
subtitle: ""
header-img:
tags:
- Effactive Java
categories:
- 개발
- Java
---
## 매개변수가 많을 때 생성자로 점층적 생성자 패턴을 사용할 경우
- 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다.
- 매개변수 각 값의 이미가 무엇인지 해깔린다.
- 매개변수가 몇개인지 주의해서 사용해야 한다.
- 클라이언트 실수로 순서를 바꿔 건내줘도 컴파일에 오류를 잡지 못하고 런타임에 잘못 동작하게 된다.

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, servings, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories) {
        this(servingSize, servings, calories, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat) {
        this(servingSize, servings, calories, fat, 0);
    }
    
    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
        this(servingSize, servings, calories, fat, sodium, 0);
    }
    
    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }
}

```

## 매개변수가 많을 때 자바빈즈 패턴을 사용할 경우 
- 객체를 생성하고 setter를 사용하여 값을 설정하는 방법이다.
- 객체 하나를 만드려면 메서드를 여러 개 호출 해야한다.
- 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태에 놓이게 된다.
- 자바빈즈 패턴에서는 클래스를 불변으로 만들수 없으며 스레드 안정성을 보장할 수 없다.


```java
public class NutritionFacts {
    private int servingSize = -1;
    private int servings    = -1;
    private int calories    = 0;
    private int fat         = 0;
    private int sodium      = 0;
    private int carbohydrate = 0;

    public NutritionFacts() {

    }
    
    public void setServingSize(int val) {
        servingSize = val
    }
    
    public void setServings(int val) {
        servings = val
    }
    
    public void setCalories(int val) {
        calories = val
    }
    
    public void setFat(int val) {
        fat = val
    }
    
    public void setSodium(int val) {
        sodium = val
    }
    
    public void setCarbohydrate(int val) {
        carbohydrate = val
    }
}
```


## 매개변수가 많을 때 빌더 패턴을 사용할 경우 
- 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다.
- 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수들을 설정한다.
- build 메서드를 호출해 필요한 불변 객체를 얻는다.

```java
public class NutritionFacts {
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;

	public static class Builder {
        // 필수 매개변수    
		private final int servingSize;
		private final int servings;

		private int calories = 0;
		private int fat = 0;
		private int carbohydrate = 0;
		private int sodium = 0;

		public Builder(int servingSize, int servings) {
			this.servingSize = servingSize;
			this.servings = servings;
		}

		public Builder calories(int val) {
			calories = val;
			return this;
		}

		public Builder fat(int val) {
			fat = val;
			return this;
		}

		public Builder carbohydrate(int val) {
			carbohydrate = val;
			return this;
		}

		public Builder sodium(int val) {
			sodium = val;
			return this;
		}

		public NutritionFacts build() {
			return new NutritionFacts(this);
		}
	}

	private NutritionFacts(Builder builder) {
		servingSize = builder.servingSize;
		servings = builder.servings;
		calories = builder.calories;
		fat = builder.fat;
		sodium = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}
}
```

### 장점 
- 불변 객체를 만들 수 있다.
- 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기에 좋다.
- 빌더 패턴은 빌더 하나로 여러 객체를 순회하면서 만들 수도 있다.
- 빌더에 넘기는 매개변수에 따라 다른객체를 만들 수도 있다.
- 일련 번호와 같은 특정 필드는 빌더가 알아서 채우도록 할 수 있다.

### 단점 
- 객체를 만드려면 빌더부터 만들어야 한다. 성능에 민감한 상황에 문제가 될 수 있다.
-  점층적 생성자 패턴보다 코드가 장황해서 매개변수가 4개 이상일 때 사용해야 효율적이다.