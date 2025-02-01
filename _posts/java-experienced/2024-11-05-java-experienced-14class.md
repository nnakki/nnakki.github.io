---
published: true
title: "[Java]Class 클래스"
excerpt: "김영한의 실전자바(중급편) - System클래스, Math, Random 클래스 등"

categories:
  - Java
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-중급편, Class, System클래스, Math, Random] 

permalink: /java-intermediate/class/

date: 2024-11-05
last_modified_at: 2024-11-05 # 최종 수정 날짜
---

# Class 클래스

자바에서 <mark>Class 클래스</mark>는 클래스의 정보(메타데이터)를 다루는데 사용된다. Class 클래스를 통해 개발자는 실행중인 자바 애플리케이션 내에서 필요한 클래스의 속성과 메소드에 대한 정보를 조회하고 조작할 수 있다. 

* **타입 정보 얻기** : 클래스의 이름, 슈퍼클래스, 인터페이서, 접근 제한자 등과 같은 정보를 조회할 수 있다.
* **리플렉션** : 클래스에 정의된 메소드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메소드를 호출하는 등의 작업을 할 수 있다. 
* **동적 로딩과 생성** : `Class.forName()` 메서드를 사용하여 클래스를 동적으로 로드하고, `newInstance()`메서드를 호출하는 등의 작업을 할 수 있다.
* **애노테이션 처리** : 클래스에 적용된 어노테이션(annotation)을 조회하고 처리하는 기능을 제공한다.

**<mark>Class 클래스의 주요 기능</mark>**

*  getDeclaredFileds() : 클래스의 모든 필드를 조회한다. 
*  getDeclaredMethods() : 클래스의 모든 메서드를 조회한다.
* getSuperclass() : 클래스의 부모 클래스르 조회한다.
* getInterface() : 클래스의 인터페이스를 조회한다. 

> **리플렉션 - reflection**
>
> Class를 사용하면 클래스의 메타 정보를 기반으로 클래스에 정의돈 메소드, 필드, 생성자 등을 조회화고 이들을 통해 객체 인스턴스를 생성하거나 메소드를 호출하는 작업을 할 수 있다. 이런 작업을 리플렉션이라고 한다. 추가로 어노테이션 정보를 읽어서 특별한 기능을 수행할 수도 있다. 최신 프레임워크들은 이런 기능을 적극적으로 활용한다.

## System 클래스

System 클래스는 시스템과 관련된 기본 기능들을 제공한다. 

```java
public class SystemMain{
  public static void main(String[] args){
    //현재시간(밀리초)를 가져온다.
    long currentTimeMillis = System.currentTimeMills();
    //현재시간(나노초)를 가져온다.
    long nanoTime = System.nanoTime();
    
    //환경변수를 읽는다. 
    System.getenv();
    
    //시스템 속성을 읽는다. 
    System.getProperties();
    System.getProperties("java.version")
    
    //배열을 고속으로 복사한다.
    char[] originalArray = new char[]{'h', 'e', 'l', 'l', 'o'};
   	char[] copiedArray = new char[5];
    System.arraycopy(originalArray, 0, copiedArray, 0, originalArray.length);
  }
}
```

1. 표준 입력, 출력, 오류 스트림 : `System.in`, `System.out`, `System.err`은 각각 표준 입력, 표준 출력, 표준 오류 스트림을 나타낸다.
2. 시간 측정 : `System.currentTimeMillis()`와 `System.nanoTime()`은 현재 시간을 밀리초 또는 나노초로 제공한다.
3. 환경변수 : `System.getenv()`메서드를 사용하여 os에서 설정한 환경 변수의 값을 얻을 수 있다.
4. 시스템 속성 : `System.getProperties()`를 사용해 현재 시스템 속성을 얻거나, `System.getProperties(String key)`로 특정 속성을 얻을 수 있다. 
5. 시스템 종료 : `System.exit(int status)` 메서드는 프로그램을 종료하고, OS에 프로그램 종료의 상태 코드를 전달한다 → <span style="color:red">권장하지 않음</span>
6. 배열 고속 복사 : `System.arraycopy`는 시스템 레벨에 최적화 된 메모리 복사 연산을 사용한다. 직접 반복문을 사용하여 배열을 복사하는 것 보다 수 배 이상 빠른 성능을 제공한다. 

## Math, Random 클래스

### 📌 Math 클래스

`Math`는 수많은 수학 문제를 해결하는 클래스다. 

* 기본연산 메서드
  * abs(x) : 절대값 
  * max(a,b) : 최대값
  * min(a,b) : 최소값
  * pow(a, b) : a의 b제곱
  * sqrt(x) : 제곱근 
  * random() : 0.0에서 1.0 사이의 무작위 값 생성
* 반올림 및 정밀도 메서드
  * ceil(x) : 올림
  * floor(x) : 내림
  * round(x) : 반올림 

### 📌 Random 클래스

랜덤의 경우 `Math.random()`을 사용해도 되지만 Random클래스를 사용하면 더욱 다양한 랜덤값을 구할 수 있다. Math.random()도 내부에서는 Random클래스가 사용된다. (Random클래스는 java.util 패키지 소속이다.)

```java
public class RandomMain {
  public static void main(String[] args){
    Random random = new Random();

    int randomInt = random.nextInt();
   	double randomDouble = random.nextDouble();
    boolean randomBoolean = random.nextBoolean();
    
    /*범위조회*/
    int randomRange = random.nextInt(10); //0~9
    int randomRange2 = random.nextInt(10)+1; //1~10
  }
}
```

