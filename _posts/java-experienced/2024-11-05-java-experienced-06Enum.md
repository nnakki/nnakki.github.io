---
published: true
title: "[Java]Enum - 열거형"
excerpt: "김영한의 실전자바(중급편) - 문자열 타입고 안정성, 타입 안전 열거형 패턴, Enum"

categories:
  - Java-Experienced
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, Enum] 

permalink: /java-experienced/enum/

date: 2024-11-06
last_modified_at: 2024-11-06 # 최종 수정 날짜
---

## 문자열과 타입 안정성(Enum의 등장 배경)

열거형을 이해하기 위해서는 근본적으로 열거형이 왜 필요한지에 대한 이유부터 알아야한다. 아래의 예시로 그 이유를 알아보자.

### ❓단순 String을 사용하면 어떤 문제가 있을까

> ✅요구사항 
>
> 고객은 3등급으로 나누고, 상품 구매시 등급별로 할인을 적용한다. (할인 시 소수점 이하는 버린다.)
>
> Basic →10%할인, Gold→20%할인, Diamond→30%할인
>
> ```java
> public class DiscountService {
>  public int discount(String grade, int price){
>      int discountPercent = 0;
>      switch (grade){
>          case "BASIC":
>              discountPercent = 10;
>              break;
>          case "GOLD":
>              discountPercent = 20;
>              break;
>          case "DIAMOND":
>              discountPercent = 30;
>              break;
>      }
>      return price * discountPercent / 100;
>  }
> }
> 
> /*메인클래스*/
> public class StringGradeMain {
>   public static void main(String[] args) {
>      int price = 10000;
>      DiscountService discountService = new DiscountService();
>      int basic = discountService.discount("BASIC", price);
>      int gold = discountService.discount("GOLD", price);
>      int diamond = discountService.discount("DIAMOND", price);
>   }
> }
> ```
위 클래스의 문제점이 무엇일까? 단순히 문자열을 입력하여 할인율을 구하는 방식은 오타가 발생하기도 쉽고, 유효하지 않은 값이 입력될 가능성이 있다. 예를 들어 **존재하지 않는 등급을 입력하거나, 오타를 내거나, 소문자를 입력하는 등** 정확한 할인율을 구할 수 없는 문자열을 입력한 상황에도 런타임 전까지 코드는 아무런 문제도 발생시키지 않는다.

위의 예시처럼  **<mark>String을 사용하는 경우 타입 안정석이 부족한 문제</mark>**가 발생하게 된다. 

* **타입 안정성 부족** : 문자열은 오타가 발생하기 쉽고, 유효하지 않은 값이 입력될 수 있다.
* **데이터 일관성** : "GOLD", "gold", "Gold" 등 다양한 형식으로 문쟈열을 입력할 수 있기 때문에 일관성이 떨어진다. 

- **컴파일 시 오류 감지 불가** : 잘못 된 값을 입력하더라도 컴파일 시에 감지되지 않고, 런타임시에만 문제가 발견되기 때문에 디버깅이 어려워 질 수 있다.

>  이러한 문제를 해결하려면 값을 특정 범위로 제한해야만한다. <u>예시로 비유하자면 정확히 BASIC, GOLD, DIAMOND라는 문자만 메서드에 전달 될 수 있도록 제한해야한다</u>는 것이다. 하지만 String 클래스는 어떤 문자열이든 받을 수 있기 때문에 자바 문법 관점에서는 이를 String으로서 해결할 수 있는 방법이 없다. 



### ❓상수를 사용한다면 해결될까?

```java
public class StringGrade {
  public static final String BASIC = "BASIC";
  public static final String GOLD = "GOLD";
  public static final String DIAMOND = "DIAMOND";
}

/*메인클래스*/
public class StringGradeMain {
  public static void main(String[] args) {
     int price = 10000;
     DiscountService discountService = new DiscountService();
     int basic = discountService.discount(StringGrade.BASIC, price);
     int gold = discountService.discount(StringGrade.GOLD, price);
     int diamond = discountService.discount(StringGrade.DIAMOND, price);
  }
}
```

그렇다면 오타나, 정확하지 않은 문자열이 들어가지 않도록 상수를 사용한다면 문제는 해결될까? 코드가 조금 더 명확해질 수는 있으나 문제를 근본적으로 해결할 수는 없다. `discount(String grade, int price)`메서드는 여전히 인자로 String을 받고 있다. 즉, 굳이 상수로 할당되어 있는 `StringGrade.basic`이 아니어도 개발 히스토리를 모르는 다른 개발자가 개발을 하면서 일반 문자열로 "VIP"등의 잘못 된 문자열을 넣어도 막을 수 없다는 것이다. 

**<mark>결국 BASIC, GOLD, DIAMOND라는 문자<span style="color:red">만</span> 전달한다는 부분의 문제를 해결하지 못한다.</mark>**

## 타입 안전 열거형 패턴 - Type-safe Enum Pattern

위의 문제들을 해결하기 위해 나온 것이 바로 타입 안전 열거형 패턴이다.

**✅ 타입 안전 열거형 패턴의 장점** 

1. **타입 안정성 향상** : 정해진 객체만 사용할 수 있기 때문에, 잘못된 값을 입력하는 문제를 근본적으로 방지할 수 있다. 다시 말해 잘못된 값이 할당되거나 사용되는 것을 컴파일 시점에 방지할 수 있다. 
2. **데이터 일관성** : 정해진 객체만 사용하므로 데이터 일관성이 보장된다. 
3. **제한 된 인스턴스 생성** : 클래스는 사전에 정의된 몇 개의 인스턴스 만 생성하고, 외부에서는 이 인스턴스들만 사용할 수 있도록 한다. 이를 통해 미리 정의된 값들만 사용하도록 보장한다.

→**그리고 자바는 이러한 타입 안전 열거형 패턴을 매우 편리하게 사용할 수 있는 열거형(Enum-Type)을 제공한다.**

## 열거형 - Enum Type

자바의 enum타입은 타입 안정성을 제공하고, 코드의 가독성을 높이며, 예상 가능한 값들의 집합을 표현하는데 사용된다. <u>열거형은 클래스로 `java.lang.Enum`을 상속받고, 외부에서 임의로 생성할 수 없다는 특징</u>을 갖는다. 

```java
public enum Grade{	/*열거형을 정의할 때는 class 대신 enum을 사용한다.*/
  BASIC, GOLD, DIAMOND	/*원하는 상수의 이름을 나열하면 된다.*/
}
```

**✅ 열거형의 장점**

1. **타입 안정성 향상** : 열거형은 사전에 정의된 상수들로만 구성되므로, 유효하지 않은 값이 입력될 가능성이 없다. 이런 경우 컴파일 오류가 발생한다.
2. **간결성 및 일관성** : 열거형을 사용하면 코드가 더 간결하고 명확해지며, 데이터의 일관성이 보장된다.
3. **확장성** : 새로운 상수를 추가하고 싶다면, ENUM에 그냥 새로운 상수를 추가하기만 하면 된다.

✅ **열거형의 특징**

1. 열거형은 `java.lang.Enum`을 자동으로 상속받는다. 이미 상속받았기 때문에, 다른 클래스 상속을 받을 수 없다.
2. 열거형은 인터페이스를 구현할 수 있다.
3. 열거형에 추상 메서드를 선언하고, 구현할 수 있다. (익명 클래스와 같은 방식을 사용한다.)

### 📌 열거형의 주요 메서드

> * `values()` : 모든 ENUM 상수를 포함하는 배열을 반환한다.
> * `valueOf(String name)` : 주어진 이름과 일치하는 ENUM 상수를 반환한다.
> * `name()` : ENUM  상수의 이름을 문자열로 반환한다.
> * `toString()` : ENUM 상수의 이름을 문자열로 반환한다. name()메서드와 유사하지만, toString()은 직접 오버라이드 할 수 있다.

