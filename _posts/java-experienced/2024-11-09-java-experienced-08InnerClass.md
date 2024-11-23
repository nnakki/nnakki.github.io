---
published: true
title: "[Java]중첩 클래스와 내부클래스"
excerpt: "김영한의 실전자바(중급편) - 중첩 클래스의 정의, 정적 중첩 클래스와 활용, 내부 클래스의 정의와 활용 등"

categories:
  - Java-Intermediate
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, 중첩 클래스, 내부 클래스, 익명 클래스] 

permalink: /java-intermediate/InnerClass1/

date: 2024-11-09
last_modified_at: 2024-11-09 # 최종 수정 날짜
---

## 중첩 클래스

```java
class Outer{
  //중첩 클래스
  class Nested{
    ...
  }
}
```

위의 그림과 같이 클래스 안에 클래스를 중첩해서 정의한 경우 이를 중첩 클래스라고 부른다.<br>중첩 클래스는 총 4가지 종류가 있고, 크게 2가지로 분류할 수 있다.

- **static** : 정적 중첩 클래스
- **non-static** 
  - 내부 클래스 → 인스턴스 변수와 같은 위치에 선언
  - 지역 클래스 → 지역 변수와 같은 위치에 선언 (코드 블록에 선언)
  - 익명 클래스 (이름 없는 클래스)

```java
class Outer{
  static class StaticNested{}  //정적 중첩 클래스	
  class Inner{}   //내부 클래스
  
  public void process(){
    int value = 0;
    class Local {} // 지역클래스
  }
}
```

**중첩 클래스와 내부 클래스는 무슨 차이일까?** 

쉽게 이야기하면 중첩은 나의 안에 있지만 내것이 아닌 것을 말한다. 단순히 위치만 안에 있다는 의미이다. 반면 내부는 나의 내부에서 나를 구성하는 요소라는 의미를 포함한다. 정리하자면 **정적 중첩 클래스**는 <u>바깥 클래스의 안에 있지만 바깥 클래스와 관계 없는 전혀 다른 클래스</u>를 말하고, **내부 클래스**는 <u>바깥 클래스의 내부에 있으면서 바깥 클래스를 구성하는 요소</u>를 말한다. 여기서 의미하는 중첩과 내부를 분류하는 핵심은 바로 <u>바깥 클래스의 입장에서 볼 때 안에 있는 클래스가 나의 인스턴스에 소속이 되는가 되지 않는가의 차이</u>다. <mark>즉, 내부 클래스들은 바깥 클래스의 인스턴스에 소속되지만, 정적 중첩 클래스는 그렇지 않다.</mark>

**중첩 클래스는 언제 사용해야 할까?** 

내부 클래스를 포함한 모든 중첩 클래스는 특정 클래스가 다른 하나의 클래스 안에서만 사용되거나, 둘이 아주 긴밀하게 연결되어 있는 특별한 경우에만 사용해야 한다. 외부의 여러 클래스가 특정 중첩 클래스를 사용한다면 중첩 클래스를 만든다. 

- **특정 클래스가 다른 하나의 클래스 안에서만 사용되는 경우** 해당 클래스 안에 포함하는 것이 논리적으로 더 그룹화 된다. 패키지를 열었을 때 다른 곳에서 사용 될 필요가 없는 클래스가 외부에 노출되지 않는 장점을 가질 수 있으므로 이런 경우 중첩 클래스를 사용한다. 
- **중첩 클래스는 바깥 클래스의 private멤버에 접근할 수 있다**. 이런 방식으로 둘을 긴밀하게 연결하면 불필요한 public메서드를 제거할 수 있다. 이러한 경우에도 중첩 클래스를 사용한다.

### 정적 중첩 클래스



클래스 앞에 `static`을 붙여서 표현한다. 자신의 멤버, 바깥 클래스의 클래스 멤버에는 접근할 수 있지만, 바깥 클래스의 인스턴스 멤버에는 접근할 수 없다. (바깥 인스턴스의 참조가 없기 때문이다.)

정적 중첩 클래스는 다른 클래스를 그냥 중첩해뒀을 뿐, 바깥 클래스와 아무런 관계가 없다. 서로 다른 클래스 2개를 서로 선언한 것이나 차이가 없으나 유일한 차이는 바깥 클래스의 <u>private 접근 제어자에 접근할 수 있다는 점이다.</u>
 ```java
  public class NestedOuter {
     	private static int outClassValue = 3;
      private int outInstanceValue = 2;
  
      static class Nested { // 정적 중첩 클래스
          private int nestedInstanceValue = 1;
          public void print(){
              System.out.println(nestedInstanceValue);
              //System.out.println(outInstanceValue); 접근불가
              System.out.println(outClassValue);
         }
     }
  }
 ```
![image-20241109180636465]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241109180636465.png){: style="width:70%;"}

NestedOuter클래스와 Nested클래스는 각각 별개의 클래스로 인스턴스가 각각 생성되는걸 볼 수 있다. **Nested인스턴스는 NestedOuter인스턴스에 접근할 수 없기 때문에 outInstanceValue에 접근할 수 없는 걸 확인할 수 있다.**

### 내부 클래스

`static`이 붙지 않으며, 바깥 클래스의 인스턴스에 소속된다.
```java
 public class InnerOuter {
     private static int outClassValue = 3;
     private int outInstanceValue = 2;
 
     class Inner{	// 내부 클래스
         private int innerInstanceValue = 1;
         public void print(){
             System.out.println(innerInstanceValue);
             System.out.println(outInstanceValue);
             System.out.println(outClassValue);
         }
     }
 }
```
![image-20241109180554966]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241109180554966.png){: style="width:50%; height:50%"}![image-20241109181432230]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241109181432230.png){: style="width:50%; height:50%"}

정적 중첩 클래스와는 다르게 바깥 클래스인 InnerOuter인스턴스 내부에 내부클래스인 Inner클래스의 인스턴스가 생기는 걸 확인할 수 있다. <u>(실제로 내부에 인스턴스가 생성되는 것은 아니고, 오른쪽 그림처럼 내부에 참조값을 보관하게끔 생기지만 개념적으로 왼쪽 그림처럼 이해해도 문제없다.)</u> **따라서 Inner클래스는 InnerOuter 인스턴스(바깥 클래스의 인스턴스) 내부에 생성 된 바깥 인스턴스 멤버인 outInstanceValue에 접근할 수 있는 걸 확인할 수 있다.**

### 지역클래스

지역클래스는 내부 클래스의 특별한 한 종류로서 내부 클래스의 특징을 그대로 가진다. 즉, 바깥 클래스의 인스턴스 멤버에 접근할 수 있다.  지역 클래스는 지역 변수와 같이 코드 블럭 안에 정의된다. 

```java
public class LocalOuterV1 {
    private int outInstanceVar = 3;

    public void process(int paramVar){
        int localVar = 1;

        class LocalPrinter {//지역클래스
            int value = 0;

            public void printData(){
                System.out.println(value);//자신의 인스턴스 변수 접근가능
                System.out.println(localVar);//코드 블럭의 지역변수 접근가능
                System.out.println(paramVar);//코드 블럭의 매개변수 접근가능
                System.out.println(outInstanceVar);//바깥 클래스의 인스턴스변수 접근가능
            }
        }
        LocalPrinter localPrinter = new LocalPrinter();
        localPrinter.printData();
    }

    public static void main(String[] args) {
        LocalOuterV1 instance = new LocalOuterV1();
        instance.process(2);
    }
}
```

#### 지역 변수 캡쳐 

아래 코드의 예시를 보며 지역 변수 캡쳐의 무엇인지 알아보자.

```java
public class LocalOuterV2 {
    private int outInstanceVar = 3;

    public Printer process(int paramVar) {
        int localVar = 1;   //지역 변수는 스택 프레임이 종료되는 순간 함께 제거된다.

        class LocalPrinter implements Printer {

            int value = 0;

            @Override
            public void print() {
                System.out.println("value = " + value);

                //인스턴스 지역 변수보다 더 오래 살아남는다.
                System.out.println("localVar = " + localVar);
                System.out.println("paramVar = " + paramVar);

                System.out.println("outInstanceVar = " + outInstanceVar);
            }
        }
        Printer printer = new LocalPrinter();
        //printer.print()를 여기서 실행하지 않고 Printer 인스턴스만 반환한다.
        return printer;
    }

    public static void main(String[] args) {
        LocalOuterV2 instance = new LocalOuterV2();
        Printer printer = instance.process(2);
        //printer.print()를 나중에 실행한다. process()의 스택 프레임이 사라진 이후에 실행
        printer.print();
    }
}
```
위 코드에서 `Printer printer = new LocalPrinter();`를 통해 LocalPrinter인스턴스를 만든 시점의 메모리 영역을 그림으로 표현하면 아래와 같다.

![image-20241110171056999]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241110171056999.png){: style="width:70%;"}

스택 영역에는 process메서드의 매개변수인 paramVar, 지역변수인 localVar, printer가 생성된다.<br>힙 영역에는 LocalOuter클래스와 LocalPrinter클래스의 인스턴스가 각각 생성되고 각 인스턴스안에는 각 클래스의 인스턴스 변수 정보가 함께 들어있다. LocalPrinter인스턴스의 경우 바깥 클래스인 LocalOuter의 참조값 정보가 함께 들어있다.

변수의 생존주기를 더하여 스택 영역의 지역변수의 생존 주기를 살펴보면 아래와 같다.

- **printer (LocalPrinter)** <br>
  → `process()`메서드 안에서 생성되지만, `main()`메서드에서 process()메서드에서 반환받은 값을 printer변수에 보관하므로 <u>**main() 메서드가 종료될때까지 생존한다.**</u>

- **paramVar, localVar**<br>→ 지역변수이므로 process() 메서드가 실행되는 동안만 존재한다. process()메서드가 종료되면 스택 프레임이 제거되면서 스택 영역에서 함께 제거된다. <br>즉, `Printer printer = instance.process(2)`가 실행되는 동안만 생존한다.

**그렇다면 위 코드만 살펴보았을 때는 의문점이 생긴다.** <br><u>main()메서드에서 print()메서드는 process()메서드보다 나중에 실행되는데, print() 메서드 내부에는 process()메서드의 지역 변수인 LocalVar와 ParamVar를 호출하게 되어있다.</u> 생존주기로는 LocalVar와 ParamVar는 이미 스택 영역에서 삭제되었는데 그렇다면 해당 지역변수들의 값을 가져올 수 있는걸까?

> **✅ 참고 - 변수의 생명주기** <br>
> ![image-20241110170112597]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241110170112597.png){: style="width:70%;"}
>
> **클래스 변수** : 클래스 변수(static)는 <u>메서드 영역에 존재</u>하고, 프로그램 종료시까지 존재한다.<br>**인스턴수 변수** : 인스턴스 변수는 <u>힙 영역에 존재</u>하고, 본인이 소속된 인스턴스가 GC가 되기 전까지 존재한다. <br>**지역 변수** : 지역 변수는 <u>스택 영역에 존재</u>하고, 메서드 호출이 끝나면 사라진다. 생존주기가 아주 짧다.

<mark>즉, 생존주기가 매우 짧은 지역 변수에, 생존 주기가 그보다 긴 인스턴스에서 접근하려고 할 때 지역변수가 이미 제거되었다면 어떻게 되는걸까?</mark>

→  자바는 이런 문제를 해결하기 위해 지역 클래스의 인스턴스를 생성하는 시점에 필요한 지역 변수를 복사해서 생성한 인스턴스에 함께 넣어둔다. 이런 과정을 **<mark>지역변수 캡쳐(Capture)</mark>**라 한다. 그림으로 표현하면 아래와 같다.

![image-20241110174204203]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241110174204203.png){: style="width:70%;"}

![image-20241110174431085]({{site.url}}/images/2024-11-09-java-experienced-08InnerClass/image-20241110174431085.png){: style="width:70%;"}

LocalPrinter 인스턴스에서 print()메서드를 통해 paramVar, localVar에 접근하면 사실은 스택 영역에 있는 지역 변수에 접근하는 것이 아니라 인스턴스에 있는 캡쳐한 변수에 접근하는 것이다. 캡쳐한 paramVar, LocalVar의 생명주기는 LocalPrinter 인스턴스의 생명주기와 같기 때문에, 각 지역변수의 생명주기와는 무관하게 언제든지 캡쳐변수에 접근할 수 있는 것이다. 이렇게 지역 변수와 지역 클래스를 통해 생성한 인스턴스의 생명주기가 다른 문제를 해결 할 수 있다.

단, 변수를 캡쳐하여 인스턴스에 담아놓기 때문에 스택 영역에 존재하는 지역 변수의 값과 인스턴스에 캡처한 캡처 변수의 값이 서로 달라지는 동기화 문제가 발생해서는 안된다. 따라서 <mark>지역 클래스가 접근하는 지역 변수는 절대로 중간에 값이 변하면 안된다. 따라서 `final`로 선언하거나 `사실상final`이어야만 한다. </mark>

### 익명 클래스

익명 클래스 역시 지역 클래스의 특별한 한 종류로서 클래스의 이름이 없다는 특징을 가진다.

지역 클래스는 사용하기 위해 선언과 생성이라는 2가지 단계를 거친다. 하지만 익명클래스를 사용하면 클래스의 이름을 생략하고 클래스의 선언과 생성을 한번에 처리할 수 있다는 장점이 있다.

```java
public class AnonymousOuter {
    private int outInstanceVar = 3;

    public void process(int paramVar){
        int localVar = 1;

        Printer printer = new Printer(){//익명클래스(선언과 생성)
            int value = 0;
            @Override
            public void print() {
                System.out.println("value = " + value);
                System.out.println("localVar = " + localVar);
                System.out.println("paramVar = " + paramVar);
                System.out.println("outInstanceVar = " + outInstanceVar);
            }
        };
    }
}
```

익명 클래스는 클래스의 본문을 정의하면서 동시에 생성한다. `new`다음에 바로 상속받으면서 구현 할 부모 타입을 입력하면 된다.

🔻**특징**

- 익명 클래스는 이름 없는 지역 클래스를 선언하면서 동시에 생성한다.
- **<u>익명 클래스는 부모 클래스를 상속 받거나 또는 인터페이스를 구현해야 한다. 즉, 상위 클래스나 인터페이스가 있어야 익명 클래스를 구현할 수 있다.</u>**
- 익명 클래스는 이름을 가지지 않으므로 생성자를 가질 수 없다.(기본 생성자만 사용된다.)

**🔻장점**

익명 클래스를 사용하면 클래스를 별도로 정의하지 않고도 인터페이스나 추상 클래스를 즉석에서 구현할 수 있어 코드가 간결해진다. 하지만 복잡하거나 재사용이 필요한 경우에는 별도의 클래스를 정의하는 편이 좋다. 

**🔻익명 클래스를 사용할 수 없는 경우**

익명 클래스는 <u>단 한번만 인스턴스를 생성할 수 있다</u>. 여러 번 생성이 필요하다면 익명 클래스를 사용할 수 없다. 대신 지역 클래스를 선언하고 사용하는 것이 좋다.

#### 익명 클래스의 활용 예시

```java
public class Ex1RefMain {
    public static void hello(Process process){
        process.run();
    }
  
  	/* 예시1 - 지역클래스를 사용한 경우 */
    public static void main(String[] args) {

        class Dice implements Process {
            @Override
            public void run() {
                int randomValue = new Random().nextInt(6)+1;
                System.out.println("주사위 = "+randomValue);
            }
        }
        Process dice = new Dice();
        hello(dice);
    }
  
  	/* 예시2 - 익명클래스를 사용한 경우 */
    public static void main(String[] args) {
        Process dice = new Process() {
            @Override
            public void run() {
                int randomValue = new Random().nextInt(6)+1;
                System.out.println("주사위 = "+randomValue);
            }
        };
        hello(dice);
    }  	

  	/* 예시3 - 익명클래스 참조값을 메서드의 인수로 직접 전달한 경우 */
    public static void main(String[] args) {
        hello(new Process() {
            @Override
            public void run() {
                int randomValue = new Random().nextInt(6)+1;
                System.out.println("주사위 = "+randomValue);
            }
        });
    }  
}
```

#### 람다(lamda)

자바8 이전까지는 메서드에 인수로 전달할 수 있는 것은 int,double과 같은 기본형 타입 혹은 Process, Member와 같은 참조형 타입(인스턴스)였다. 하지만 **자바8 이후로 메서드(함수)를 인수로 전달할 수 있게 되었는데 이를 람다라고 한다.**

```java
  /* 예시4 - hello(runDice()메서드);*/  
	public static void main(String[] args) {
        hello(()->{
            int randomValue = new Random().nextInt(6)+1;
            System.out.println("주사위 = "+randomValue);
        });
    }
```

