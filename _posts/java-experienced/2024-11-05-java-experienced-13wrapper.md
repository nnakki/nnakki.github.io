---
published: true
title: "[Java]래퍼 클래스"
excerpt: "김영한의 실전자바(중급편) - 기본형의 한계, 자바 래퍼 클래스, 오토 박싱, 주요 메서드와 성능"

categories:
  - Java
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, 래퍼 클래스, wrapper] 

permalink: /java-intermediate/wrapper/

date: 2024-11-05
last_modified_at: 2024-11-05 # 최종 수정 날짜
---

# 래퍼 클래스

## 기본형의 한계

자바는 객체 지향 언어지만 자바 내에 int, doulbe과 같은 기본형은 객체가 아니므로 다음과 같은 한계를 지니게 된다.

1. **객체가 아님** : 기본형 데이터는 객체가 아니기 때문에, 객체 지향 프로그래밍의 장점을 살릴 수 없다. 예를 들자면 메서드를 제공할 수 없다던가 하는 문제이다. 추가로 객체 참조가 필요한 컬렉션 프레임워크도 사용할 수 없고, 제네릭도 사용할 수 없다.
2. **null값을 가질 수 없다** : 때로는 데이터 없음 이라는 상태를 표현해야 할 수도 있는데 기본형은 항상 값이 들어있으므로 이런 상태를 표현할 수 없다.

## 자바 래퍼 클래스

위와 같은 한계를 극복하기 위해서 자바는 기본형을 객체로 감싸서 더 편리하게 사용할 수 있는 래퍼 클래스를 제공한다. 다시 말해서 래퍼 클래스는 기본형의 객체 버전이다. 

```markdown
byte → `Byte`
short → `Short`
int → `Integer`
long → `Long`
float → `Float`
double → `Double`
char → `Character`
boolean → `Boolean`
```

래퍼 클래스는 `불변`이며, `equals()`를 통하여 비교하여야한다. 

### 📌 래퍼 클래스의 활용

```java
package lang.wrapper;

public class WrapperClassMain {

    public static void main(String[] args) {
      	Integer newInteger = new Integer(10);
        Integer integerObj = Integer.valueOf(10); 
        Long longObj = Long.valueOf(100);
        Double doubleObj = Double.valueOf(10.5);

        System.out.println("newInteger = " + newInteger);
        System.out.println("integerObj = " + integerObj);
        System.out.println("longObj = " + longObj);
        System.out.println("doubleObj = " + doubleObj);

        System.out.println("내부 값 읽기");
        int intValue = integerObj.intValue();
        System.out.println("intValue = " + intValue);
        long longValue = longObj.longValue();
        System.out.println("longValue = " + longValue);

        System.out.println("비교");
        System.out.println("==: " + (newInteger == integerObj));
        System.out.println("equals: " + (newInteger.equals(integerObj)));
      	// == : false
      	// equals : true
    }
}

```

<mark>박싱(Boxing)</mark> :  기본형을 래퍼클래스에 넣는 과정을 마치 박스에 넣는 것과 같다고 하여 박싱이라고 부른다.

> 📍 **참고 : Integer.valueOf()** 
>
> 내부 코드를 살펴보면 위 코드의 newInteger를 만드는 것과 동일하게 new Integer(i)로 객체를 생성하는 걸 알 수 있다. 그럼에도 new Integer(n)로 객체를 생성하는 것이 아니라 Integer.valueOf()가 권장되는 이유는 캐싱에 있다. Integer.valueOf()에는 성능 최적화 기능이 있어, 개발자들이 일반적으로 자주 사용하는 -128 ~ 127 범위의 Integer 클래스를 미리 생성해준다. 해당 범위의 값을 조회하면 미리 생성해둔 Integer 값을 재사용하기 때문에 효율적이다. 

intValue() - 언박싱 (래퍼 클래스에 들어있던 기본형의 값을 다시 꺼내는 과정)

비교에는 <mark>equals( )</mark>를 사용한다 → 래퍼 클래스는 객체이므로 == 비교의 경우 객체의 참조값을 비교한다. 논리적 등등성을 비교하기 위해서 equals를 통해 값을 비교해야한다.

### 📌 오토 박싱 (Autoboxing)

자바에서 int를 Integer로 변환하거나, Integer를 int로 변환하는 과정은 위의 예시코드와 같이, valueOf()메서드(`박싱`)와 intValue()(`언박싱`) 메서드를 사용한다. 하지만 Integer와 같은 경우 기본형과 래퍼클래스를 상호 변환하는 일이 매우 자주 발생하고, 이를 매번 이렇게 메서드를 사용해서 변환하는 과정은 매우 번거로웠기 때문에 자바는 아래와 같은 오토박싱이라는 기능을 제공한다. 

```java
int value = 10;
Integer boxedValue = value;
int unboxedValue = boxedValue;
```

이렇듯 개발자는 위와 같이 적어도, 자바 컴파일러가 개발자를 대신해 컴파일 과정에서valueOf()와 intValue()의 코드를 자동으로 추가해주는 것이다. 

### 📌 주요 메서드와 성능 

#### * 주요 메서드

```java
Integer.valueOf(10); / Integer.valueOf("10") // 숫자,문자열을 래퍼 객체로 반환
Integer.parseInt("10") // 문자열 전용, 기본형 변환
Integer.compareTo // 비교
Integer.sum(10,20) // 산술연산, static으로 제공됨.
Integer.max(10,20)
Integer.min(10,20)
```

#### * 성능

래퍼 클래스는 객체이기 때문에 기본형보다 다양한 기능을 제공한다. 그렇다면 왜 기본형을 제공해야 할까? 이유는 기본형과 래퍼 클래스의 성능 차이 때문이다. 

예를들어 1 ~ 1,000,000,000 까지의 숫자를 전부 더하는 반복문을 돌린다면 기본형이 래퍼 클래스에 비해 훨씬 빠른 연산 속도를 보여준다. 이유는 다음과 같다.

기본형은 메모리에서 단순히 그 크기만큼의 공간을 차지한다. int로 예를 들자면 기본형은 보통 4바이트의 메모리를 사용한다. 하지만 래퍼클래스의 경우 객체를 생성하기 때문에, 기본형의 값 뿐만 아니라, 객체 자체를 다루기 위한 메타데이터가 포함되기 때문에 기본형에 비해 대략 8~16바이트 정도의 더 많은 메모리를 차지하게 된다. 이렇듯 메모리를 더 차지하는 만큼 연산의 성능은 저하되게 되는 것이다.

**<mark>그렇다면 언제 기본형 클래스를 사용하고, 언제 래퍼 클래스를 사용해야할까?</mark>**

> 기본형 클래스와 래퍼 클래스의 연산에 대략 5배의 연산 속도의 차이가 난다고 하더라도, 일반적인 애플리케이션을 만드는 관점에서는 이러한 차이는 매우 미미하다. 따라서 CPU연산을 아주아주 많이 수행하는 특수한 겨우나, 수만-수십만 이상의 연속된 연산을 수행하는 정도에나 기본형을 사용한 최적화를 고려할 만 하다는 것이다. 이 외의 일반적인 경우에는 코드의 유지보수 측면에서 더 나은 것을 선택하여 사용하면 된다.

