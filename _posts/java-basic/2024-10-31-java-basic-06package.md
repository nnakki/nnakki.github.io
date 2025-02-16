---
published: true
title: "[Java]패키지와 접근제어자"
excerpt: "김영한의 실전자바(기본편) - 패키지와 접근제어자"

categories:
  - Java
tags: # 포스트 태그
  - [김영한의 실전 자바-기본편] 

permalink: /java-basic/package/

date: 2024-10-31
last_modified_at: 2024-10-31 # 최종 수정 날짜
---
# 패키지

컴퓨터가 파일을 분류하기 위해 폴더, 디렉터리라는 개념을 제공하듯이 자바는 패키지를 제공한다. 
```markdown
* user
  ㄴ User
  ㄴ UserManager
  ㄴ UserHistory
* product
  ㄴ Product
  ㄴ ProductCatalog
  ㄴ ProductImage
* order
  ㄴ Order
  ㄴ OrderService
  ㄴ OrderHistory
* cart
  ㄴ ShoppingCart
	ㄴ CartItem	
* payment
  ㄴ Payment
  ㄴ PaymentHistory
* shipping
  ㄴ Shipment
  ㄴ ShipmentTracker
```
소문자로 시작하는 `user`, `product`등이 바로 패키지이다. 해당 패키지 안에 관련된 자바 클래스를 넣으면 된다.

패키지를 사용하는 경우 <u>항상 코드 첫줄에 `package 패키지명` 과 같이 패키지 이름을 적어주어야 한다</u>.

> - **사용자와 같은 위치** : 예를 들어 A클래스와 B클래스가 pack1 이라는 동일한 패키지에 소속 된 경우, 패키지 경로는 생략해도 된다. 
>
> - **사용자와 다른 위치** : A클래스는 pack1에 C클래스는 pack2 라는 서로 다른 패키지에 속한 경우, pack1.A.User 와 같이 패키지 전체 경로를 포함해서 클래스를 적어주어야 사용할 수 있다.

위와 같이 패키지의 위치가 다른 경우 pack.a.User와 같이 항상 전체 경로를 적어주는 것은 매우 불편하기 때문에 `import`를 활용하여 이러한 작업을 편하게 사용할 수 있다.

```java
package pack2;

import pack1.a.User;

public class C {
  public static void main(String[] args){
    C c = new C();
    A a = new A();
  }
}
```

이렇듯 `import`를 사용하면 서로 다른 패키지의 클래스를 사용할 수 있다. 

즉 패캐지 덕분에 클래스 이름이 같아도 패키지의 이름으로 구부하여 같은 이름의 클래스를 사용할 수 있다는 것이다. 
<br>그렇다면 클래스 이름이 중복 된 경우 이를 구분해서 사용할 대는 어떻게 해야하는가?

```java
package pack;
import pack.a.User;

public class PackageMain3 {
  public static void main(String[] args) {
  User userA = new User();
  pack.b.User userB = new pack.b.User();
}
```

같은 이름의 클래스가 있다면 `import` 는 둘중 하나만 선택할 수 있다. 이때는 자주 사용하는 클래스를 `import` 하고 나머지를 패키지를 포함한 전체 경로를 적어주면 된다. 물론 둘다 전체 경로를 적어준다면 `import` 를 사용하지 않아도 된다.

## 패키지 규칙
1. 패키지의 이름과 위치는 폴더(디렉토리)의 위치와 같아야한다. ⭐️필수⭐️
2. 패키지의 이름은 모두 소문자를 사용한다 (관례)
3. 패키지 이름의 앞 부분에는 일반적으로 회사의 도메인 이름을 거꾸로 사용한다. ex) `com.company.myapp`
   - 필수는 아니지만 오픈소스나 라이브러리를 만들어서 외부에 제공하는 경우 꼭 지키는 것이 좋다.

#### 패키지와 계층 구조 
- a
  - b
  - c

위와 같은 패키지 구조가 있을 때, `a`, `a.b`,`a.c`이렇게 총 3개의 패키지가 존재하게 된다. <br>계층 구조상 a패키지 하위에 b,c패키지가 존재하지만 java에서는 이런 구조를 고려하지 않는다.<br>따라서 a 패캐지의 클래스에서 b,c 패키지의 클래스가 필요하면 반드시 `import`하여 사용하여야 하고 반대의 경우도 마찬가지이다. 정리하자면 패키지가 계층 구조를 이루더라도 모든 패키지는 서로 다른 패키지이다. 

## 접근 제어자

자바는 `pulblic`,`private`같은 접근 제어자를 제공한다. 접근 제어자를 사용하면 해당 클래스 외부에서 특정 필드나 메서드에 접근하는 것을 허용하거나 제한할 수 있다. 

접근 제어자는 왜 필요한가?

<span style="font-size:13px;">스피커에 들어가는 소프트웨어를 개발해야한다. <br>단, 스피커의 음량은 절대로 100을 넘으면 안되는 요구사항이 있다.</span>

```java
package access;

public class Speaker {
  int volume;
  Speaker(int volume) {
    this.volume = volume;
  }
  void volumeUp() {
    if (volume >= 100) {
      System.out.println("음량을 증가할 수 없습니다. 최대 음량입니다.");
    } else {
      volume += 10;
      System.out.println("음량을 10 증가합니다.");
    }
  }
}

public class SpeakerMain {
  public static void main(String[] args) {
    Speaker speaker = new Speaker(90);
    speaker.showVolume();
    speaker.volumeUp();
    speaker.showVolume();
    speaker.volumeUp();
    speaker.showVolume();
    
    //필드에 직접 접근
    System.out.println("volume 필드 직접 접근 수정");
    speaker.volume = 200;
    speaker.showVolume();
  }
}
```
위 예시에서 Speaker 객체는 요구사항인 volume 100을 넘지 못하도록 메서드를 만들었지만, 해당 객체를 만들어 사용하는 SpeakerMain 클래스에서 Speaker의 volume필드에 직접 접근하여 값을 조정할 수 있기 때문에 요구사항을 위반할 수 있는 가능성이 생겼다.

![image-20241031201323309](https://nnakki.github.io/images/2024-10-31-%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98%20%EC%8B%A4%EC%A0%84%20%EC%9E%90%EB%B0%94(%EA%B8%B0%EB%B3%B8%ED%8E%B8)%20-%20%ED%8C%A8%ED%82%A4%EC%A7%80%EC%99%80%20%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4%EC%9E%90/image-20241031201323309.png)

<span style="color:red">이런 문제를 근본적으로 해결하기 위해서는 volume 필드의 외부 접근을 차단하는 방법이 필요한 것이다. </span>

```java
  private int volume;
```

`private` 이 붙은 경우 해당 클래스 내부에서만 접근할 수 있다. 즉, 모든 외부 호출을 막는다. <br>이렇듯 접근제어자를 사용하여 접근의 허용범위를 제한하면서 후에 생길 수 있는 문제를 방지할 수 있다. 

### 접근제어자의 종류

**접근 제어자의 핵심은 속성과 기능을 외부로 숨기는 것이다.**

- `private` : 모든 외부 호출을 막는다.<br>나의 클래스 안으로 속성과 기능을 숨길 때 사용
- `default` : 같은 패키지 안에서의 호출은 허용한다. (package-private)<br>나의 패키지 안으로 속성과 기능을 숨길 때 사용
- `protected` : 같은 패키지 안에서의 호출은 허용한다. 패키지가 달라도 상속 관계의 호출은 허용한다.<br>상속관계로 속성과 기능을 숨길 때 사용.
- `public` :  모든 외부 호출을 허용한다.

#### 접근 제어자의 사용 위치

**접근 제어자는 필드와 메서드, 생성자에 사용**된다. <br>추가로 클래스 레벨에도 일부 접근 제어자를 사용할 수 있다. 

### 접근 제어자의 사용

#### 필드, 메서드 레벨

#### 클래스 레벨 
클래스 레벨의 접근제어자는 `public`, `default`만 사용할 수 있다. <br>public 클래스는 반드시 파일명과 이름이 같아야 한다.
- 하나의 자바 파일에 `public` 클래스는 하나만 등장할 수 있다. 
- 하나의 자바 파일에 `default` 접근 제어자를 사용하는 클래스는 무한정 만들 수 있다.

### <span style="color:#4d70a5">📌 캡슐화</span> 

> **캡슐화**는 데이터와 해당 데이터를처리하는 메서드를 하나로 묶어서 외부에서의 접근을 제한하는 것을 일컫는다.<br>캡슐화를 통해 데이터의 직접적인 변경을 방지하거나 제한할 수 있다. 쉽게 이야기 하자면 속성과 기능을 하나로 묶고, 외부에 꼭 필요한 기능만 노출하고 나머지는 모두 내부로 숨기는 행위다.

**객체 지향에서는 어떤 것을 숨기고 어떤 것을 노출해야할까?**

1. **데이터를 숨겨야 한다.**

   캡슐화에서 가장 필수로 숨겨야하는 것은 속성(데이터)다. 객체 내부의 데이터를 외부에서 함부로 접근하게 두면, 클래스 안에서 데이터를 다루는 모든 로직을 무시하고 데이터를 변경할 수 있다. 따라서 캡슐화가 깨지게 된다. <br><span style="color:red;">⭐️⭐️즉, 객체의 데이터는 객체가 제공하는 기능인 메서드를 통해서 접근해야 한다.⭐️⭐️</span>

2. **기능을 숨겨야 한다.**

   객체의 기능 중 외부에서는 사용하지 않고 내부에서만 사용하는 기능들이 있을 것이다. 이런 기능 또한 모두 감추는 게 좋다. 사용자의 입장에서 꼭 필요한 기능만 외부로 노출하고 나머지 기능은 모두 내부로 숨겨야한다.

> **데이터는 모두 숨기고, 꼭 필요한 기능만 노출하는 것이 좋은 캡슐화이다.**
