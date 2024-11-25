---
published: true
title: "[Spring Project]테스트 코드 실습"
excerpt: "Junit5, 테스트 코드 실습"

categories:
  - Spring-Projects
tags: # 포스트 태그
  - [테스트 코드, JUnit5] 

permalink: /spring-projects/testcode/

date: 2024-11-23
last_modified_at: 2024-11-23 # 최종 수정 날짜

---

# 객체지향 패러다임

## 테스트 코드 실습

- 자바 단위 테스팅 프레임 워크 : [JUnit5](https://junit.org/junit5/docs/current/user-guide/#writing-tests) 사용

- 테스트 코드 가독성을 높여주는 자바 라이브러리 : [AssertJ](https://assertj.github.io/doc/#assertj-core-assertions-guide)

> 테스트 코드를 작성하는 이유? 
>
> 문서화 역할, 코드의 결함을 발견하기 위함, 리팩토링 시 안정성 확보<br>테스트 하기 쉬운 코드를 작성하다 보면 더 낮은 결합도를 가진 설계를 쉽게 얻을 수 있음

### TDD (Test Drvien Development) 테스트 주도개발

프로덕션 코드보다 테스트 코드를 먼저 작성하는 개발방법.<br>TFD(Test First Development)+리팩토링 

### BDD(Behavior Driven Development) 행위 주도 개발

시나리오 기반으로 테스트코드를 작성하는 개발 방법<br>하나의 시나리오는 Given, When, Then 구조를 가진다. 

### 실습 - 비밀번호 유효성 검증기

**✅ 요구사항** 

: 비밀번호는 최소 8자 이상 12자 이하여야한다.<br>비밀번호가 8자 미만 또는 12자 초과인 경우 IllegalArgumentException을 발생시켜야 한다.<br>경계조건에 대해 테스트 코드를 작성해야 한다.

---

## 객체지향?

적절한 객체에게 적절한 책임을 할당하여 서로 메세지를 주고 받으면서 협력하도록 하는것. <br>점점 증가하는 소프트웨어 복잡도를 낮추기 위해 객체지향 패러다임이 대두되고 있다.<br>

**📌 클래스가 아닌 객체에 초점을 맞춰야한다.**

**📌 각 객체에 얼마나 적절한 역할과 책임을 할당하는지가 중요하다.**

### 객체지향 프로그래밍의 4가지 특징

1. 추상화(Abstraction) : 불필요한 부분을 제거함으로써, 필요한 부분만 남겨둔다. 복잡성을 낮추기 위한 도구로 사용.
2. 다형성(Polymorphism) : 하나의 타입으로 여러 타입의 객체를 참조할 수 있는 것.
3. 캡슐화(Encapsulation) : 객체 내부의 세부 사항을 외부로부터 감추는 것 -> 인터페이스만 공개하여 변경하기 쉬운 코드를 만들고자함.
4. 상속(Inheritance) :  

### 객체지향 설계의 5원칙 (SOLID)

1. SRP (Single Responsibility Principle) - 단일책임의 원칙

   : 하나의 메서드는 하나의 책임만을 가져야한다.

2. OCP (Open-Closed Principle) - 개방 폐쇄의 원칙
   : 확장에는 열려있고, 변경에는 닫혀있다. 즉, 기존 코드를 변경하지 않고 기능을 추가할 수 있어야한다.

3. LSP (Liskov's Subsitution Principle) - 리스코프 치환의 원칙 
   : 상위 타입의 객체를 하위 타입의 객체로 치환하여도 동작에 문제가 없어야한다.

4. ISP (Interface Segregation Principle) - 인터페이스 분리의 원칙
   : 많은 기능을 가진 인터페이스를 작은 단위로 분리시키므로서 클라이언트에게 필요한 인터페이스들만 구현하도록 한다.<br>클라이언트가 사용하지 않는 기능에 의존하게 되면 예상하지 못한 문제가 발생할 수 있는데 이러한 문제를 예방한다.

5. DIP (Dependency Inversion Principle) - 의존성 역전의 원칙 

   : 의존관계를 맺을 때, 변경이 자주 일어나지 않는 쪽에 의존해야 한다. 

### ❓어떻게 설계하는 게 좋을까

1. 도메인을 구성하는 객체에 어떤 것들이 있는지 고민한다.
2. 객체들 간의 관계를 고민한다.
3. 동적인 객체를 정적인 타입으로 추상화해서 도메인을 모델링한다.
4. 협력을 설계한다.
5. 객체들을 포괄하는 타입에 적절한 책임을 할당한다.
6. 구현한다.

### 실습 - 사칙연산 계산기

**✅ 요구사항** 

: 간단한 사칙 연산이 가능해야한다.<br>양수로만 계산할 수 있다.<br>나눗셈에서 0을 나누는 경우 IllegalArgument 예외를 발생시킨다.<br>MVC패턴(Model-View-Controller) 기반으로 구현한다.

---





