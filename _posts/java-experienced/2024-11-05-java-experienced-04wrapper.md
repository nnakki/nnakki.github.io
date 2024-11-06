---
published: true
title: "[Java]래퍼 클래스"
excerpt: "김영한의 실전자바(중급편) - 기본형의 한계, 자바 래퍼 클래스, 오토 박싱, 주요 메서드와 성능"

categories:
  - Java-Experienced
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, 래퍼 클래스, wrapper] 

permalink: /java-experienced/wrapper/

date: 2024-11-05
last_modified_at: 2024-11-05 # 최종 수정 날짜
---

# 래퍼 클래스

## 기본형의 한계

자바는 객체 지향 언어지만 자바 내에 int, doulbe과 같은 기본형은 객체가 아니다. 그렇기 때문에 다음과 같은 한계를 지니게 된다.

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

### 래퍼 클래스의 활용

```java
package lang.wrapper;

public class WrapperClassMain {

    public static void main(String[] args) {
        Integer newInteger = new Integer(10); //미래에 삭제 예정, 대신에 valueOf()를 사용
        Integer integerObj = Integer.valueOf(10); //-128 ~ 127 자주 사용하는 숫자 값 재사용, 불변
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
    }
}

```

