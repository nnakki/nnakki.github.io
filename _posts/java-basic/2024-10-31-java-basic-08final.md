---
published: true
title: "[Java]final"
excerpt: "김영한의 실전자바(기본편) - final"

categories:
  - Java
tags: # 포스트 태그
  - [김영한의 실전 자바-기본편] 

permalink: /java-basic/final/

date: 2024-10-31
last_modified_at: 2024-10-31 # 최종 수정 날짜
---

# final 변수와 상수

변수에 `final`키워드가 붙으면 더는 값을 변경할 수가 없다.<br>참고로 `final`은 클래스, 메서드를 포함한 여러 곳에 붙을 수 있지만, 이 글에선 변수에 붙는 키워드에 대해서만 알아본다.

## final - 지역변수

```java
public class FinalLocalMain{
  public static void main(String[] args){
    //final 지역변수
    final int data1;
    data1 = 10; // 최초 한번만 할당 가능
    //data1 = 20; // 컴파일 오류
  }
  
  //final 매개변수
  static void method(final int parameter){
  		//parameter = 20; 
      /*컴파일 오류 (매개변수로 들어오는 parameter에서 최초 할당이 됨. 메서드 내부에서 재할당 불가)*/
  }
}
```

: final을 지역변수에 설정할 경우 최초 한번만 할당이 가능하다. 이후에 변수의 값을 변경하려면 컴파일 오류가 발생한다. 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용된다.

## final - 필드(멤버변수)

```java
public class ConstructInit{
  final int value;
  
  public ConstructInit(int value){
    this.value = value;
  }
}
```

`final`을 필드에서 사용할 경우 해당 필드는 생성자를 통해서 한번만 초기화 될 수 있다. 

```java
//final 필드 - 필드 초기화
public class FieldInit{
  static final int CONST_VALUE = 10;
  final int value = 10;
}
```

final필드를 필드에서 초기화하면 이미 값이 설정되었기 때문에 생정자를 통해서도 초기화 할 수 없다. <br>코드에서 보는 것 처럼 static필드에도 final을 선언할 수 있다.

<span style="color:#4d70a5">**ConstructInit**</span> 클래스와 같이 생성자를 사용해서 final 필드를 초기화 하는 경우, 각 인스턴스마다 final 필드에 다른 값을 할당할 수 있다. 물론 final을 사용했기 때문에 이후 이 값을 변경하는 것은 불가능하다. 

<span style="color:#4d70a5">**FieldInit**</span>과 같이 final필드를 필드에서 초기화하는 경우, 모든 인스턴스는 같은 값을 가진다. 왜냐하면 생성자 초기화와 다르게 필드 초기화는 필드의 코드에 해당 값이 미리 정해져있기 때문이다. 모든 인스턴스가 같은 값을 사용하기 때문에 결과적으로는 메모리를 낭비하게 된다. (JVM에서 내부 최적화를 시도할 수 있다.) 따라서 이럴 때 사용하면 좋은 것이 static이다.

이런 이유로 **static final**은 단 하나만 존재하는 변하지 않는 고정된 값을 갖게 되는데 이를 '**<u>상수</u>**'라고 한다.

> * 애플리케이션 안에는 다양한 상수가 존재할 수 있다. 수학, 시간 등등 실생활에서 사용하는 상수부터, 애플리케이션의 다양한 설정을 위한 상수들도 있다.
>
> * 보통 이런 상수들은 애플리케이션 전반에서 사용되기 때문에 `public` 를 자주 사용한다. 물론 특정 위치에서만
>
> * 사용된다면 다른 접근 제어자를 사용하면 된다.
>
> * 상수는 중앙에서 값을 하나로 관리할 수 있다는 장점도 있다.
>
> * 상수는 런타임에 변경할 수 없다. 상수를 변경하려면 프로그램을 종료하고, 코드를 변경한 다음에 프로그램을 다시 실행해야 한다.

## final 변수와 참조

`final` 은 변수의 값을 변경하지 못하게 막는다. <br>`final` 을 기본형 변수에 사용하면 값을 변경할 수 없다.<br>`final` 을 참조형 변수에 사용하면 참조값을 변경할 수 없다.