---
published: true
title: "[Java]String클래스"
excerpt: "김영한의 실전자바(중급편) - 불변 객체, String클래스의 메서드와 특징, String 최적화, 메서드 체이닝(Method Chaining)"

categories:
  - Java-Experienced
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, String] 

permalink: /java-experienced/String/

date: 2024-11-04
last_modified_at: 2024-11-04 # 최종 수정 날짜
---

# String 클래스 

## 1️⃣ String 클래스 기본 

String을 통해 문자열을 생성하는 방법

```java
String str1 = "hello";
String str2 = new String("hello");
```

String은 int, boolean과 같은 기본형이 아니라 '클래스'다. 따라서 str변수에는 참조값만 들어갈 수 있으나 첫번째 코드와 같이 "hello"; 라는 문자열이 직접 들어갈 수 있다. 문자열은 매우 자주 사용되기 때문에 편의상 쌍따옴표로 문자열을 감싸면 자바 언어에서 알아서 new String("hello");와 같이 변경해준다.

- **String 클래스의 구조 (속성과 기능을 가진다)**

  ```java
  public final class String {
    //문자열 보관
    private final char[] value;// 자바 9 이전
    private final byte[] value;// 자바 9 이후
    .
    .
    //여러 메서드
    public String concat(String str) {...}
    public int length() {...}
    ...
  }
  ```

  `private final char[] value;` : String의 실제 문자열 값이 보관된다. 문자 데이터 자체는char[]에 보관되는 것이다. String 클래스는 개발자가 직접 다루기 불편한 char[] 을 내부에 감추고 편리하게 문자열을 다룰 수 있도록 하는 다양한 메서드들을 제공해준다. 

- **String 클래스와 참조형** 

  ```java
  String a = "hello";
  String b = " java";
  String result1 = a.concat(b);
  String result2 = a + b;
  ```

  String은 클래스로 기본적으로 `+`와 같은 연산이 불가능하다. 하지만 문자열은 너무 자주 다루어지기 때문에 + 연산을 제공한다.

## 2️⃣ String 클래스의 비교 

 <span style="color:red">String 클래스는 항상 ==이 아니라 .equals()로 비교해야한다. 즉, 동일성이 아니라 동등성을 비교해야한다.</span>

```java
/* str1 == str2 → false, str1.equals(str2) → true */
String str1 = new String("hello");
String str2 = new String("hello");

/* str3 == str4 → true, str3.equals(str4) → true */
String str3 = "hello";
String str4 = "hello";
```

참고로 String 클래스는 클래스 내부에 문자열 값을 비교할 수 있도록 Object클래스의 equals()메서드를 재정의 해두었다. 그치만 위의 예시코드에서 왜 str3 == str4 의 값이 true가 되는 걸까?

>  **문자열 풀**
> ![image-20241104212418479]({{site.url}}/images/2024-11-04-java-experienced-03string/image-20241104212418479.png){: style={"width:60%, height:60%"}
>
> `String str3 = "hello";` 처럼 문자열 리터럴을 직접 사용하는 경우, 자바는 메모리의 효율성과 성능 최적화를 위해 문자열 풀을 생성한다. 
>
> 자바가 실행되는 시점에 클래스에 문자열 리터럴이 있으면 문자열 풀에 `String`인스턴스를 미리 만들어둔다. 이 때 같은 문자열이 있다면 새로 만들지 않는다.
>
> 따라서 `String str4 == "hello";`의 경우 문자열 리터럴을 직접 사용하였으므로 문자열 풀을 먼저 찾게 되는데 기존에 문자열 풀에 만들어진 hello가 있기 때문에 동일한 hello를 바라보게 되고, 결과적으로 같은 참조값을 같게 되어 `==`비교에서 true를 반환하게 된다.
>
> 문자열 풀 덕분에 같은 문자를 사용하는 경우 메모리 사용을 줄이고 문자를 만드는 시간도 줄어들기 때문에 성능도 최적화 할 수 있다.  (참고로 문자열 풀은 Heap 영역을 사용한다.)

## 3️⃣ String 클래스의 불변 객체

String은 불변 객체이다. 따라서 생성 이후에 절대로 내부의 문자값을 변경할 수 없다. 

```java
String str = "hello";
str.concat("java");
System.out.println(str); // hello
```

str은 String 객체로 불변 객체이기 때문에 concat("java")로 문자열을 이어붙였다해도 기존의 str의 값이 변경되지 않는다. 해당 코드를 올바르게 작동시키기 위해선 `String str2 = str.concat("java");`와 같이 새로운 불변 객체를 만들어 값을 받아주어야 한다.

그렇다면 `String`은 왜 불변 객체로 설계되었을까? 앞서 정리한 불변 객체의 장점 이외에도 String의 경우는 `문자열 풀`을 추가적인 이유로 들 수 있다. 

String에서 리터럴 값을 그대로 쓰는 경우 이미 문자열 풀에 생성된 문자열의 참조값을 바라본다. 그런데 만약 String의 값이 내부에서 변경되게 된다면 기존에 문자열 풀에서 같은 문자를 참조하는 변수의 모든 문자가 함께 변경되어 버리는 문제가 발생하게 되는 것이다. 

이러한 이유로 String 클래스는 불변으로 설계되어서 이러한 사이드 이펙트의 발생을 방지한다.

## 4️⃣ String 클래스의 주요 메서드

자주 사용하는 기능 위주로 정리해보았다. 

1. **문자열 정보 조회**
   - length() : 문자열의 길이반환
   - isEmpty() : 문자열이 비어 있는지 확인 (길이가 0)
   - isBlang() : 문자열이 비어 있는지 확인 (길이가 0 또는 모든 문자열이 공백)
   - charAt(int index) : 지정된 인덱스의 문자를 반환 
2. **문자열 비교**
   - equlas(Object anObject) : 두 문자열이 동일한지 비교한다.
   - equlasIgnoreCase(String str) : 문자열을 대소문자의 구분없이 비교한다.
   - compareTo(String str) : 문자열을 사전 순을 비교한다.
   - compareToIgnoreCase(String str) : 문자열을 사전 순으로 대소문자의 구분없이 비교한다.
   - startsWith(String prefix)  : 문자열이 특정 접두사로 시작하는지 확인
   - endsWith(String unifix) : 문자열이 특정 접미사로 끝나는지 확인
3. **문자열 검색**
   - contains(char s) : 문자열이 특정 문자열으 포함하는지
   - indexOf(String ch) / indexOf(String ch, int fromIndex) :  문자열이 처음 등장하는 위치를 반환한다.
   - lastIndexOf(String ch) : 문자열이 마지막으로 등장하는 위치를 반환한다.
4. **문자열 조작 및 변환**
   - subString(int beginIndex) / subString(int beginIndex, int endIndex) : 문자열의 부분 문자열을 반환한다.
   - concat(String str) : 문자열을 더한다.
   - replace(CharSequence target, CharSequence replacement) : 특정 문자열을 새 문자열로 대체한다.
   - replaceAll(String regex, String replacement) :  문자열에서 정규표현식과 일치하는 부분을 새 문자열로 대체한다.

5. **문자열 분할 및 조합**
   - split(String regex)` : 문자열을 정규 표현식을 기준으로 분할한다.
   - Join() : 주어진 구분자로 여러 문자열을 결합한다.

6. **기타 유틸리티**
   - valueOf(Object of) : 다양한 타입을 문자열로 변환한다.
   - toCharArray() : 문자열을 배열로 변환한다. 
   - format(String format, Object... args) : 형식 문자열과 인자를 사용하여 새로운 문자열을 생성한다. 
   - matches(String regex) : 문자열이 주어진 정규 표현식과 일치하는지 확인한다.

## 5️⃣ StringBuilder - 가변 String

불변인 String 클래스는 문자열을 더하거나 변경할 때 마다 계속해서 새로운 객체를 생성해야 한다는 단점이 있다. 문자를 자주 변경해야 하는 상황이라면 더 많은 String 객체를 만들고, GC해야한다. 결과적으로 컴퓨터의 CPU, 메모리 자원을 더 많이 사용하게 된다. 그리고 문자열의 크기가 클수록, 자주 변경할수록 시스템의 자원은 더 많이 소모된다. 

#### StringBuilder  

이러한 문제를 해결하기 위해 가변 String인 StringBuilder 객체를 제공한다. 

```java
public final class StringBuilder {
  char[] value;// 자바 9 이전
  byte[] value;// 자바 9 이후
  //여러 메서드
  public StringBuilder append(String str) {...}
  public int length() {...}
  ...
}
```

보이는 것과 같이 StringBuilder 클래스의 필드는 final이 아니므로 변경이 가능하다.  메모리 사용을 줄이고 성능을 향상 시킬 수 있지만 언제나 그렇듯 사이드 이펙트를 주의해야한다. 따라서 StringBuilder는 보통 문자열을 변경하는 동안에만 사용하고, 문자열 변경이 끝나면 안전한 불변 객체인 String으로 변환하여 사용하는 것이 좋다.

## 6️⃣ String 최적화 

자바 컴파일러는 위에 기술했던 것 처럼 문자열 리터럴을 더하는 부분을 자동으로 합쳐준다. 따라서 런타임에 별도의 문자열 결합 연산을 수행하지 않기 때문에 성능이 향상된다.

```java
String helloWorld = "Hello"+", world"
//컴파일 후 
String helloworld = "Hello, world"
```

하지만 문자열 변수의 경우 그 안에 어떤 값이 들어있는지 컴파일 시점에는 알 수 없기 때문에 위와 같이 단순히 합칠 수 없다.  이 경우에는 예를 들면 다음과 같이 최적화를 수행한다. ( 이 방식은 자바 버전에 따라 달라진다.)

```java
String result = str1 + str2;
String result = new StringBuilder().append(str1).append(str2).toString();
```

이렇듯 자바가 최적화를 처리해주므로 간단한 경우에는 굳이 `StringBuilder`를 직접 사용하지 않아도 되고, 문자열에 + 연산을 사용하면 충분하다. 하지만 String 최적화가 어려운 경우가 있는데 이럴 때는 개발자가 StringBuilder를 사용해주는 편이 좋다. 어떤 경우가 있을까? 

* **반복문에서 반복해서 문자를 연결할 때** 

  반복문 루프 내부에서 최적화가 되는 것 처럼 보이겠지만 ,반복 횟수만큼 객체를 생성해야 한다. 반복문 내에서의 연결은 런타임에 연결할 문자열의 개수와 내용이 결졍되므로 컴파일러는 얼마나 많은 반복이 일어날 지, 각 반복에서 문자열이 어떻게 변할 지 예측할 수 없다. 따라서 이런 상황에서는 최적화가 매우 어려운 것이다. 

* **조건문을 통해서 동적으로 문자열을 연결할 때** 

* **복잡한 문자열의 특정 부분을 변경해야 할 때**

* **매우 긴 대용량 문자열을 다뤄야 할 때** 

> **✅ 참고 : StrinbBuilder vs StringBuffer**
>
> StringBuilder와 StringBuffer는 똑같은 기능을 수행하는 클래스다.
>
> * **StringBuffer** :  내부에 동기화가 되어있어, 멀티 스레드 상황에 안전하지만 동기화 오버헤드로 인해 성능이 느리다.
>
> * **StringBuilder** : 멀티 스레드 상황에 안전하지 않지만, 동기화 오버헤드가 없으므로 속도가 빠르다.

## 7️⃣ 메서드 체이닝(Method Chaining)

```java
public class ValueAdder{
  private int value;
  
  public ValueAdder add(int addValue){
    value += addValue;
    return this; //자기 자신을 반환한다.
  }
  
  public int getValue(){
    return value;
  }
}

public class ChainingMain{
  public static void main(String[] args){
    Valueadder adder = new ValueAdder();
    adder.add(1);
    adder.add(2);
    adder.add(3);  
    
    System.out.println(adder.getValue());// 실행결과:6
  }
}

public class ChainingMain2{
  public static void main(String[] args){
    ValueAdder adder = new ValueAdder();
		int result = adder.add(1).add(2).add(3).getValue();

    System.out.println(adder3.getValue());// 실행결과:6
  }
}
```

위 코드의 차이를 알아보자.<br>ChainingMain클래스는 add()메서드를 여러번 호출하여 값을 누적하여 더한 후 출력한다. 하지만 반환값(자기자신)을 사용하지는 않았다.

ChainingMain2클래스는 add()메서드의 반환값을 사용한다. 메서드 호출의 결과로 자기 자신의 참조값을 반환하면, 반환된 참조값을 사용해서 메서드 호출을 계속 이어갈 수 있다. 이 모양이 마치 메서드가 체인으로 연결된 것 같다 하여 메서드 체이닝이라고 부른다. 

**메서드 체이닝 기법은 코드를 간결하고 읽기 쉽게 만들어준다.**

→ StringBuilder가 바로 이 메서드 체이닝 기법을 사용한다. 예를 들어 아래와 같다.

```java
StringBuilder sb = new StringBuilder();
sb.append("A").append("B").append("C").append("D")
  .insert(4,"java")
  .delete(4,8)
  .reverse()
  .toString()
```

