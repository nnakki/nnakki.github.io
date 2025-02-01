---
title: "[Java]기본형과 참조형"
excerpt: "김영한의 실전자바(기본편) - 기본형과 참조형 개념정리"

categories:
  - Java
tags: # 포스트 태그
  - [java, 김영한의 실전 자바-기본편] 

permalink: /java-basic/PrimitiveType/

date: 2024-10-29 # 작성 날짜
---

> 변수의 데이터 타입은 사용하는 값을 직접 변수에 넣을 수 있는 기본형과 객체가 저장 된 메모리의 위치를 가르키는 참조값을 넣을 수 있는 참조형으로 분류할 수 있다.

## 기본형(Pirmitive Type)
int, long, doble, boolean 처럼 변수에 사용할 값을 직접 넣을 수 있는 데이터 타입을 기본형이라고 한다.

## 참조형(Reference Type)
Student student1, int[] students 처럼 데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입을 참조형이라한다. 객체 또는 배열에 사용한다. 즉, 참조형 변수를 통하여 뭔가 하려면 결국 참조값을 통해 해당 위치로 이동해야 한다.

## 기본형 vs 참조형
### 1. 기본
**기본형**은 숫자 10, 20과 같이 실제 사용하는 값을 변수에 담을 수 있다. 그래서 해당 값을 바로 사용할 수 있다. 기본형은 자바가 기본으로 제공하는 데이터 타입으로 기본형은 개발자가 새로 정의할 수 없다.

**참조형**은 실제 사용하는 값을 변수에 담는 것이 아니다. 실제 객체의 위치(참조, 주소)를 저장한다. 참조형에는 객체와 배열이 있다.
  - cf) String : 자바에서 String은 특별하다. String은 사실 클래스로 참조형이다. 하지만 기본형처럼 문자값을 바로 대입할 수 있다. 문자는 자바에서 매우 자주 다루기 때문에 자바에서 특별히 편의 기능을 제공하고 있다.

### 2. 계산
**기본형**은 들어있는 값을 그대로 계산할 수 있다.
<br>**참조형**은 들어있는 참조값을 그대로 사용할 수 없다.

### 3. 변수 대입  
<span style="color:red">**대원칙 : 자바는 항상 변수의 값을 복사하여 대입한다**</span>
<br>기본형은 변수에 들어있는 실제 사용하는 값을 복사해서 대입한다.
  - 기본형 변수 대입 예시
```java
public class VarChange1 {
	public static void main(String[] args) {
		int a = 10;
		int b = a;
		
		System.out.println("a = " + a);  // a=10
		System.out.println("b = " + b);  // b=10
		
		//a 변경
		a = 20;
		System.out.println("변경 a = 20");
		System.out.println("a = " + a);  // a=20
		System.out.println("b = " + b);  // b=10
		
		//b 변경
		b = 30;
		System.out.println("변경 b = 30");
		System.out.println("a = " + a);  // a=20
		System.out.println("b = " + b);  // b=30
	}
}
```

참조형은 변수에 들어있는 참조값을 복사하여 대입한다.
<br>쉽게 이야기 하자면, 건물이 복사 되는 것이 아니라 건물의 위치인 주소만 복사된다고 생각할 수 있다. 
같은 건물이 늘어나는 게 아니라 같은 건물을 찾아갈 수 있는 방법이 하나 늘어나는 것 뿐이다.
  - 참조형 변수 대입 예시
```java
public class VarChange2 {
	public static void main(String[] args) {
		Data dataA = new Data();
		dataA.value = 10;
		Data dataB = dataA;
		
		System.out.println("dataA 참조값=" + dataA);  // ref.Data@x001
		System.out.println("dataB 참조값=" + dataB);  // ref.Data@x001
		System.out.println("dataA.value = " + dataA.value);  // 10
		System.out.println("dataB.value = " + dataB.value);  // 10
		
		//dataA 변경
		dataA.value = 20;
		System.out.println("변경 dataA.value = 20");
		System.out.println("dataA.value = " + dataA.value);  // 20
		System.out.println("dataB.value = " + dataB.value);  // 20
		
		//dataB 변경
		dataB.value = 30;
		System.out.println("변경 dataB.value = 30");
		System.out.println("dataA.value = " + dataA.value);  // 30
		System.out.println("dataB.value = " + dataB.value);  // 30
	}
}
```

### 4.메서드 호출 
<span style="color:red">**대원칙 : 자바는 항상 변수의 값을 복사하여 대입한다**</span>
<br>변수 대입과 같다. 메서드 호출 시 사용하는 매개변수(파라미터) 역시 변수일 뿐이다. 따라서 메서드를 호출할 때 매개변수에 값을 전달하는 것도 앞서 설명한 내용과 같이 값을 복사해서 전달한다.
<br>자바에서 메서드의 매개변수(파라미터)는 항상 값에 의해 전달된다. 그러나 이 값이 실제 값이냐, 참조값이냐에 따라 동작이 달라진다.

**기본형** : 메서드로 기본형 데이터를 전달하면 해당 값이 복사되어 전달된다. 이 경우, 메서드 내부에서 매개변수(파라미터)의 값을 변경해도, 호출자의 변수 값에는 영향이 없다.

  - 기본형과 메서드 호출 예시
```java    
public class MethodChange1 {
	public static void main(String[] args) {
		int a = 10;
			
		System.out.println("메서드 호출 전: a = " + a);  // a=10
		changePrimitive(a);
		
		System.out.println("메서드 호출 후: a = " + a);  // a=10
	}
	
	static void changePrimitive(int x) {
		x = 20;
	}
}
```

**참조형 :** 메서드로 참조형 데이터를 전달하면 참조값이 복사되어 전달된다. 이 경우, 메서드 내부에서 매개변수(파라미터)로 전달된 객체의 멤버 변수를 변경하면, 호출자의 객체도 변경된다.
  - 참조형 메서드 호출 예시
	```java
	package ref;
	
	public class MethodChange2 {
		public static void main(String[] args) {
			Data dataA = new Data();
			dataA.value = 10;
			
			System.out.println("메서드 호출 전: dataA.value = " + dataA.value); // 10
			changeReference(dataA);
			
			System.out.println("메서드 호출 후: dataA.value = " + dataA.value); // 20
		}
		
		static void changeReference(Data dataX) {
			dataX.value = 20;
		}
	}
	```

# 변수의 초기화

### 변수의 종류

- 멤버 변수(필드) : 클래스에 선언
- 지역 변수 : 메서드에 선언, 매개변수도 지역 변수의 한 종류이다.

### 변수의 값 초기화

**멤버 변수 : 자동 초기화**
  - 인스턴스의 멤버 변수는 인스턴스를 생성할 때 자동으로 초기화가 된다.
  - 숫자 = 0, boolean = false, 참조형 = null
  - 개발자가 초기값을 직접 지정할 수 있다.

**지역 변수 : 수동 초기화**
  - 지역 변수는 항상 직접 초기화해야 한다.

# null

> 참조형 변수에는 항상 객체가 있는 위치를 가리키는 참조값이 들어간다. <br>그런데 아직 가리키는 대상이 없거나, 가리키는 대상을 나중에 입력하고 싶다면 어떻게 해야할까? <br>참조형 변수에서 아직 가리키는 대상이 없다면 `null` 이라는 특별한 값을 넣어둘 수 있다. <br>`null` 은 값이 존재하지 않는, 없다는 뜻이다

```java
public class NullMain1 {
	public static void main(String[] args) {
		Data data = null;
		System.out.println("1. data = " + data); // null
		
		data = new Data();
		System.out.println("2. data = " + data); // x001
		
		data = null;
		System.out.println("3. data = " + data); // null
	}
}
```

`data` 에 `null` 을 할당하면 앞서 생성한 `x001` `Data` 인스턴스를 더는 아무도 참조하지 않는다. 이렇게 아무도 참조하지 않게 되면 `x001` 이라는 참조값을 다시 구할 방법이 없다. 따라서 해당 인스턴스에 다시 접근할 방법이 없다. 이렇게 아무도 참조하지 않는 인스턴스는 사용되지 않고 메모리 용량만 차지할 뿐이다. 자바는 아무도 참조하지 않는 인스턴스가 있으면 JVM의 GC(가비지 컬렉션)가 더이상 사용하지 않는 인스턴스라 판단하고 해당 인스턴스를 자동으로 메모리에서 제거해준다. 객체는 해당 객체를 참조하는 곳이 있으면, JVM이 종료할 때 까지 계속 생존한다. 그런데 중간에 해당 객체를 참조하는 곳이 모두 사라지면 그때 JVM은 필요 없는 객체로 판단다고 GC(가비지 컬렉션)를 사용해서 제거한다
<br>`null` 값을 가진 변수를 참조한다면 `NullPointerException`을 발생시킨다.