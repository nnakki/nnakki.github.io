---
published: true
title: "[Java]Object클래스"
excerpt: "김영한의 실전자바(중급편) - java.lang~ , Object클래스와 다형성, Object와 OCP, toString(), equals()"

categories:
  - Java-Experienced
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, Object, toString(), equals()] 

permalink: /java-experienced/ObejectClass/

date: 2024-11-05
last_modified_at: 2024-11-05 # 최종 수정 날짜
---

# Java.lang.패키지

> 자바 언어를 이루는 가장 기본이 되는 클래스들을 보관하는 패키지 
- `Object` : 모든 자바 객체의 부모 클래스
- `String` : 문자열
- `Integer, Long, Doulbe`: 래퍼 타입, 기본형 데이터 타입을 객체로 만든 것
- `Class` : 클래스 메타 정보 
- `System` : 시스템과 관련된 기본 기능들을 제공

Java.lang.패키지는 모든 자바의 애플리케이션에서 자동으로 import되기 때문에 별도로 import해주지 않아도 된다.

## Object 클래스

![image-20241104003248512]({{site.url}}/images/2024-11-05-java-experienced-object/image-20241104003248512.png){: style="width:50%; height:50%;"}

Object클래스는 모든 자바 객체의 부모 클래스기 때문에 위 그림과 같이 부모가 없는 클래스는 항상 묵시적으로는 Object 클래스를 자동으로 상속받게 된다. (ex.그림의 Parent 클래스). Object클래스가 최상위 부모 클래스인 건 아래의 예시로 확인 할 수 있다. 

><span style="font-size:14px; color:gray;">**ObjectClass.toString()**</span>
>![image-20241104003816139]({{site.url}}/images/2024-11-05-java-experienced-object/image-20241104003816139.png)<br>
><span style="font-size:14px; color:gray;">위 그림을 보면 toString()은 Object 클래스의 메소드다. 자식 클래스가 호출되면 메모리에 부모 클래스도 함께 생성된다. 따라서 new Child()를 호출하면 메모리에는 Child의 부모 클래스인 Parent와 Parent의 부모클래스(묵시적)인 Object클래스가 함께 생성되므로 Object클래스의 메소드인 toString()을 사용할 수 있다. </span>

### ❓자바에서 Object 클래스가 최상위 부모 클래스인 이유는 무엇일까?
- **공통 기능 제공**

  객체의 정보를 제공하고, 이 객체가 다른 객체와 같은지 비교하고, 객체가 어떤 클래스로 만들어졌는지 확인하는 기능은 모든 객체에 필요한 기본적인 기능이다. 이런 기본적인 기능을 객체를 만들 때마다 항상 새로운 메서드를 정의해서 만들어야한다면 상당히 비효율적이다.  그렇기 때문에 Object클래스는 아래와 같은 모든 객체에 필요한 공통 기능을 제공한다. 

  - toString() : 객체의 정보 제공
  - equals() : 객체의 같음을 비교
  - getClass() : 객체의 클래스 정보를 제공하는 기능 등

- **다형성의 기본 구현**

  부모는 자식을 담을 수 있으므로 Object클래스는 모든 객체를 참조할 수 있다. 즉, Object클래스는 다형성을 지원하는 기본적인 메커니즘을 제공할 수 있다. 모든 자바 객체는 Object 타입으로 처리될 수 있으므로 다양한 타입의 객체를 통합적으로 처리할 수 있게 해주는 것이다. 

### Object 다형성

![image-20241104141556009]({{site.url}}/images/2024-11-05-java-experienced-object/image-20241104141556009.png){: style="width:50%; height:50%;"}

```java
package lang.object.poly;
public class ObjectPolyExample1 {

    public static void main(String[] args) {
        Dog dog = new Dog();
        Car car = new Car();
        action(dog);
        action(car);
    }
    private static void action(Object obj) {
        //obj.sound(); //컴파일 오류, Object는 sound()가 없다.
        //obj.move(); //컴파일 오류, Object는 move()가 없다.

        //객체에 맞는 다운캐스팅 필요
        if (obj instanceof Dog dog) {
            dog.sound();
        } else if (obj instanceof Car car) {
            car.move();
        }
    }
}
```

위 예시에서 Dog, Car 클래스는 서로 아무 관련성은 없지만 둘 다 별도의 부모가 없으므로 Object클래스를 상속받는다. Object는 어떤 객체이든 인자로 전달할 수 있기 때문에 action()메소드에서 Object객체를 매개변수로 사용함으로서 메서드 중복을 피할 수 있다.

#### ⚠️Object 다형성의 한계 

단 Object를 통해 전달 받은 객체를 호출하려면 각 객체에 맞는 다운캐스팅 과정이 필요하다.<br>다형성을 제대로 활용하려면 <u>다형적 참조 + 메서드 오버라이딩</u>을 함께 사용해야한다. Object는 모든 객체를 대상으로 다형적 참조는 가능하나, 하위 클래스에 정의 된 다른 객체의 메서드가 정의되어 있지는 않기 때문에 메서드 오버라이딩을 활용할 수는 없다. 결국 각 객체의 기능을 호출하기 위해선 다운캐스팅을 해야한다. 물론 Object 클래스 본인이 보유한 toString() 같은 메서드는 당연히 오버라이딩이 가능하다.

그럼 이런 Object 클래스는 어떤 경우에 활용될 수 있을까?

### Object 클래스의 활용 - 1.toString()

> `Object.toString()` 메서드는 객체의 정보를 문자열 형태로 제공한다. 그래서 디버깅과 로깅에 유용하게 사용된다.
>
> 1. <span style="font-size:14px;"> `Objec.toString()` 메서드는 기본적으로 패키지를 포함한 객체의 이름과 객체의 참조값(해시코드)를 16진수로 제공한다.</span>
> 2. <span style="font-size:14px;">toString()메서드와 System.out.println()메서드의 결과는 같다.  이는 사실 System.out.println() 메서드 내부에서 Object.toString()메서드를 호출하여 사용하고 있기 때문이다.</span>

- **toString() 클래스의 오버라이딩**

`Object.toString()` 메서드가 클래스 정보와 참조값을 제공하지만 이 정보만으로는 객체의 상태를 적절히 나타내지 못한다. 그래서 보통 메서드를 재정의(오버라이딩)해서 보다 유용한 정보를 제공하는 것이 일반적이다.

### Object 클래스의 활용 - 2.Object OCP

Object 클래스가 없다면 위의 예시에서 객체를 정보를 구하는 메서드를 만들고자 할 때, Dog, Car등의 클래스에 각각 정보를 구하는 메서드를 만들고, 호출해야 했을 것이다. 각 클래스를 공통적으로 묶을 수 없기 때문에 클래스가 10개라면, 10개, 100개라면 100개의 메서드가 필요해지는 것이다. 하지만 Object 클래스라는 모든 클래스를 묶어줄 수 있는 추상적인 개념이 존재하고, 해당 클래스 안에 정보를 조회할 수 있는 toString()이라는 메서드가 존재하기 때문에 정보를 조회하는 부분에서는 단 1개의 메서드만을 불러서 이를 해결할 수 있게 된것이다.  <u>Object클래스를 사용함으로써 우리는 구체적인 개념이 아닌 추상적인 개념에 의존</u>할 수 있게 된 것이다. <br>
→ **즉, Object 클래스를 사용하면 우리는 다형적 참조, 메서드 오버라이딩을 활용하여 OCP원칙을 잘 지킬 수 있다.** 

> 📍**참고 - 정적 의존관계 vs 동적 의존관계** 
>
> - 정적 의존관계는 컴파일 시간에 결정되며 주로 클래스 간의 관계를 의미한다. 프로그램을 실행하지 않고, 클래스 내에서 사용하는 타입들만을 보고도 쉽게 의존관계를 파악할 수 있다.
> - 동적 의존관계는 프로그램을 실행하는 런타임에 확인할 수 있는 의존관계이다. print(object obj)처럼 object타입을 매개변수로 넘기는 메서드가 있는 경우, 프로그램을 실행하기 전까지 어떤 타입의 인스턴스가 넘어올 지 알수 수 없다. 이렇게 런타임에 어떤 인스턴스를 사용하는지 나타낸 것이 동적 의존관계이다. 
>
> = 보통 의존관계, ~에 의존한다 라는 표현을 사용할 때의 의존관계는 정적 의존관계를 뜻한다.



### Object 클래스의 활용 - 3.equals() : 동일성과 동등성 

Object 클래스는 동등성 비교를 위한 equals() 메서드를 제공한다. 

자바는 두 객체가 `같다`라는 표현을 2가지로 분리해서 제공한다. 

- 동일성(Iedntity) : `==`연산자를 사용해서 두 객체의 참조가 동일한 객체를 가리키고 있는지 확인, `완전히 같음`을 비교한다. <br>쉽게 얘기하여 물리적으로 같은 메모리에 있는 객체 인스턴스인지 참조값을 확인하는 것이다.
- 동등성(Equality) : `equals()`메서드를 사용하여 두 객체가 논리적으로 동등한지 확인. 보통 사람이 생각하는 논리적인 기준에 맞추어 비교한다. 

```java
//ex1
User a = new User("id-100");
User b = new User("id-100");
//ex2
String a = "hello";
String b = "hello";
```

**위 각 예시에서 a와 b는 동등하지만 동일하지 않다.**<br>물리적으로 a와 b는 서로 다른 메모리 영역에 존재하므로 참조값이 다르지만, 논리적으로는 같은 회원번호, 같은 문자열을 갖고있기 때문이다.

> - `Object.equals()` 메서드는 **기본적으로 동일성 비교를 제공**하게 된다. 
>
> ```java
> public boolean equals(Object obj) {
> 	return (this == obj);
> }
```
> 위의 예시처럼 equals로 비교하게 되는 객체가 문자열을 기준으로 동등성을 비교할지, 회원 번호를 기준을 동등성을 비교할지 알 수 없기 때문이다. 따라서 equals()로 동등성을 비교하기 위해선 각 기준에 맞게 메서드를 오버라이딩하여야 한다.
```

#### → equals() 구현

ex) id(고객번호, 문자열)이 같으면 같은 객체로 판단하도록 오버라이딩

```java
@Override
public boolean equals(Object obj) {
  UserV2 user = (UserV2) obj;
  return id.equals(user.id);
}
// 보다 정확한 구현 
@Override
public boolean equals(Object o) {
  if (this == o) return true;
  if (o == null || getClass() != o.getClass()) return false;
  User user = (User) o;
  return Objects.equals(id, user.id);
}
```

**⚠️  equals()메서드를 구현할 때 지켜야하는 규칙**

1. 반사성(Reflexivity) : 객체는 자기 자신과 동등해야 한다. `x.equals(x) → true`
2. 대칭성(Symmetry) : 객체가 서로가 동등하다고 판단된다면, 이는 양방향으로 동일해야 한다. (x.equals(y) → true 라면 y.equals(x) → true)
3. 추이성(Transitivity) : 객체 a와 b가 동일하고, 객체 b와 c가 동일하다면 객체 a와 c도 동일해야한다. 
4. 일관성(Consistency) : 비교한 두 객체의 상태가 변경되지 않는다면 `equals()`메소드는 항상 동일한 값을 반환해야 한다.
5. null에 대한 비교 : 모든 객체는 null과 비교하였을 때는 `false`를 반환해야 한다.

**⚠️ 동등성 비교가 항상 필요하지는 않기 때문에 필요한 경우에만 equals()를 재정의하면 된다. (보통 IDE가 재정의해주는 equals()를 사용한다)** 

**⚠️ equals()메서드는 보통 hashCode()와 함께 사용한다.** 
