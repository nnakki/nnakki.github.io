---
published: true
title: "다형성-1"
excerpt: "김영한의 실전자바(기본편) - 다형성, 다형적 참조, 메서드 오버라이딩"

categories:
  - Java-Basic
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-기본편, 다형성] 

permalink: /java-basic/김영한의-실전-자바(기본편)-다형성/

date: 2024-11-01
last_modified_at: 2024-11-03 # 최종 수정 날짜
---

# 다형성(1)

객체지향 프로그래밍의 대표적인 특징에는 캡슐화, 상속, 다형성 등이 있다. <br>캡슐화와 상속은 직관적으로 이해하기 쉬운 반면, 다형성은 제대로 이해하는 것도 잘 활용하는 것도 어렵다. 

다형성(Polymorphism)은 이름 그대로 "다양한 형태", "여러 형태"를 뜻한다.<br>프로그래밍에서 다형성은 한 객체가 여러 타입의 객체로 취급될 수 있는 능력을 뜻한다. 보통 하나의 객체는 하나의 타입으로 고정되어 있지만, '다형성'을 사용하면 하나의 객체가 다른 타입으로 사용 될 수 있다는 뜻이다. 

다형성을 이해하기 위해서는 2가지 핵심 이론을 알아야한다.[]()

- **다형적 참조**
- **메서드 오버리이딩**

## 다형적 참조

부모 타입의 변수가 자식 인스턴스 참조

![image-20241101171628571]({{site.url}}/images/2024-11-01-java-basic-polymorphism/image-20241101171628571.png){: style="width:30%; height:30%;" }

- 부모 타입의 변수가 자식 인스턴스를 참조한다. `Parent poly = new Child()`
- `new Child()`로 Child 인스턴스를 생성하면, 메모리 상에 Child와 Parent가 모두 생성되므로 생성 된 참조값을 Parent 타입의 변수에 담을 수 있다. <br><span style="color:red">즉, 부모는 자식 타입을 담을 수 있다.</span>
- 하지만 new Parent()로 Parent 인스턴스를 생성하면, 메모리 상에는 Parent 객체만 생성되기 때문에 생성 된 참조값을 Child 타입의 변수로 담을 수는 없다.<br><span style="color:red">즉, 자식은 부모 타입을 담을 수 없다.</span>

이렇듯, 부모 타입은 자신은 물론이고 자신을 기준으로 모든 자식 타입을 참조할 수 있다. 이것을 다양한 형태를 참조한다라고 하여 **<u>다형적 참조</u>**라고 한다.

### ⚠️ 다형적 참조의 한계
![image-20241101172558995]({{site.url}}/images/2024-11-01-java-basic-polymorphism/image-20241101172558995.png)

<span style =  "color:red; font-size=15px">Parent poly  = new Child() 이렇게 자식을 참조한 상황에서, poly가 자식 타입인 Child에 있는 메서드를 호출할 수 있을까?</span>

poly는 Parent 타입이므로 Parent 클래스부터 시작해서 필요한 기능을 찾는다. 상속 관계에서 부모 방향으로 올라가는 건 가능하지마나 자식 방향으로 내려올 수는 없기 때문에
Parent 타입의 자식 타입인 Child의 메서드를 찾을 수 없기 때문에 컴파일 오류가 발생한다.  

하지만 new Child() 로 Child 객체도 생성되어 있는데 Child 클래스의 메서드를 정말 쓸 수 없는걸까?

**→ 호출하는 타입을 자식인 Child 타입으로 변경하면 인스턴스 Child에 있는 childMethod()를 사용할 수 있다.** 이런 기능을 `캐스팅`이라 부른다. <br>(+) 부모를 자식으로 변환하는 캐스팅 기법을 `다운캐스팅`이라고 한다. (반대로 자식을 부모로 변환하는 캐스팅은 `업캐스팅`이라고 표현한다.)

### 다형성과 캐스팅

```java
package poly.basic;
  public class CastingMain1 {
    public static void main(String[] args) {
    //부모 변수가 자식 인스턴스 참조(다형적 참조)
    Parent poly = new Child();
    //단 자식의 기능은 호출할 수 없다. 컴파일 오류 발생
    //poly.childMethod();
    //다운캐스팅(부모 타입 -> 자식 타입)
    Child child = (Child) poly;
    child.childMethod();
  }
}
```

위 과정에서, 캐스팅을 한다고 하여 Parent poly 자체의 타입이 변하는 것은 아니다. poly의 참조값을 꺼내고, 꺼낸 참조값이 참고로 캐스팅을 한다고 해서 `Parent poly` 의 타입이 변하는 것은 아니다. 해당 참조값을 꺼내고 꺼낸 참조값이 Child타입이 되는 것이다. poly의 타입은 Parent 타입으로 유지된다.

### 캐스팅의 종류

#### 일시적인 다운캐스팅

```java
Child child = (Child) poly
child.childMethod();
```

위와 같이 캐스팅을 하려고 할 때, child 처럼 다운캐스팅한 변수를 담아두는 과정이 번거롭다고 느껴질 수 있다. 이런 과정없이 바로 일시적으로 다운캐스팅을 하여 인스턴스에 있는 하위클래스의 기능을 바로 사용할 수 있는 방법이 있는데 이를 **<u>일시적인 다운 캐스팅</u>**이라고 한다.

```java
//일시적 다운캐스팅 - 해당 메서드를 호출하는 순간만 다운캐스팅
((Child) poly).childMethod();
```

#### 업캐스팅
```java
package poly.basic;
  //upcasting vs downcasting
  public class CastingMain3 {
    public static void main(String[] args) {
    Child child = new Child();
    
    Parent parent1 = (Parent) child; //업캐스팅은 생략 가능, 생략 권장
    Parent parent2 = child; //업캐스팅 생략
    
    parent1.parentMethod();
    parent2.parentMethod();
    }
}
```

<span style="color:red">업캐스팅은 생략이 가능하고, 문법상 생략하는 걸 권장한다.</span> 

#### ⚠️ 다운캐스팅 사용 시 주의점

**다운캐스팅을 잘못하면 심각한 런타임 오류를 발생시킬 수 있다.**

```java
Parent parent2 = new Parent();
Child child2 = (Child) parent2; //런타임 오류 - ClassCastException
child2.childMethod(); //실행 불가
```

parent2 인스턴스는 Parent 객체로 애초에 부모 객체를 만들었기 때문에 메모리 상에는 Parent의 인스턴스만 생성되고 Child 인스턴스는 생성되지 않는다. 즉 메모리 상에 존재하지 않는 인스턴스에 대해 다운캐스팅이 되었기 때문에, <u>ClassCastException이라는 예외를 발생</u>시키게 된다.

**→ 업캐스팅은 이런 문제를 절대로 발생시키지 않는다.** 

객체를 생성하는 경우 해당 타입의 상위 부모 타입이 있다면, 부모 타입은 메모리상에 전부 함께 생성되기 때문이다. 따라서 위로만 타입을 변경하는 업캐스팅은 메모리 상에 인스턴스가 모두 존재하기 때문에 항상 안전한 것이다. 반면 다운 캐스팅은 인스턴스에 존재하지 않는 하위 인스턴스를 참조할 가능성이 있기 때문에 개발자가 이런 문제를 인지하고 있다는 의미로 항상 **명시적 캐스팅**을 사용해주어야 한다.

### instanceOf

다형성에서 참조형 변수는 여러 자식이 존재하는 경우, 다양한 자식을 대상으로 참조할 수 있다. 그럼 이 다양한 자식 중 어떤 인스턴스를 참조하고 있는 지는 어떻게 알 수 있을까? 변수가 참조하는 **인스턴스의 타입을** **확인**하고 싶다면 `instanceof` 키워드를 사용한다.

```java
public class CastingMain(){ 
  public static void main(String[] args){
    Parent parent1 = new Parent();
    Parent parent2 = new Child();
  }

  private static void call(Parent parent){
    parent.parentMethod();
    //Child 인스턴스인 경우 childeMethod() 실행
    if(paretn instatnceof Child){
      System.out.println("This is Child Instance");
      Child child = (Child) parent;
      child.childMethod();
    }
  }
}
```

**이렇듯 다운캐스팅을 수행하기 전에는 먼저 `instanceof`를 사용하여 원하는 타입으로 변경이 가능한지 확인한 다음에 다운캐스팅을 수행하는 것이 안전하다.**

#### ✅  Pattern Matching for instanceof

자바16 이후 버전부터는 `instanceof`를 사용하면서 동시에 변수를 선언할 수 있다. 이로 인해 인스턴스가 맞는 경우 직접 다운캐스팅을 하는 코드를 생략할 수 있다.<br>위 코드의 call 메서드 부분에서 이러한 문법을 활용하는 예시는 다음과 같다.

```java
private static void call(Parent parent){
  parent.parentMethod();
  if(paretn instatnceof Child child){ // 1. instanceof 키워드 뒤에 변수를 선언함.
    System.out.println("This is Child Instance");
    // Child child = (Child) parent; // 2. 직접 다운 캐스팅을 하는 코드를 생략할 수 있다. 
    child.childMethod();
  }
```

## 다형성과 메서드 오버라이딩

<span style="color:red">오버라이딩에서 꼭 기억해야할 점은 **<u>오버라이딩 된 메서드가 항상 우선권을 가진다는 점</u>**이다.</span>
![image-20241103164349698]({{site.url}}/images/2024-11-01-java-basic-polymorphism/image-20241103164349698.png){: style="width:30%; height:30%;" }
- <span style="font-size : 14px;">Parent와 Child 클래스 모두 value라는 같은 멤버 변수를 갖고있다.</span>
- -  <span style="font-size : 14px;">Parent와 Child 모두 method()라는 같은 메서드를 갖고있다.</span>
```java
public class OverridingMain {
    public static void main(String[] args) {
        //부모 변수가 자식 인스턴스 참조(다형적 참조)
        Parent poly = new Child();
        System.out.println("Parent -> Child");
        System.out.println("value = " + poly.value); //변수는 오버라이딩X
        poly.method(); //메서드 오버라이딩!
    }
}
```
poly 변수는 Parent 타입으로 poly.value, poly.method() 를 호출하게 되면 Parent 인스턴스의 기능을 찾아서 실행한다. 
- <span style="font-size : 14px;">`poly.value` : Parent 타입의 value값을 호출한다. 
- <span style="font-size : 14px;">`poly.method`: Parent 타입의 method()를 실행시키려고 하나, 하위타입인 Child.method()가 오버라이딩 되어 있다. **오버라이딩 된 메서드는 항상 우선권을 가지므로 Child.method()를 실행**하게 된다.

