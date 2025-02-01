---
title: "[Java]일급컬렉션(First-Class Collection)"
excerpt: "일급컬렉션의 의미와 활용 예시, 장단점"

categories:
  - Java
tags: # 포스트 태그
  - [일급컬렉션] 

permalink: /java/firstCollection

date: 2024-11-24 # 작성 날짜
---

# 일급 컬렉션

초격차 패키지 강의를 들으면서 객체 지향 관련 코드를 실습하는 과정에서, 일급컬렉션을 사용하여 코드를 리팩토링 하는 과정이 나왔기에 정리해두고자 한다.

> **일급 컬렉션의 정의와 특징 📌** 
>
> 다른 멤버 변수 없이 Collection 만을 Wrapping한 상태를 말하며 다음과 같은 특징을 가진다.<br>컬렉션 객체는 변수나 매개변수에 할당할 수 있다.<br>컬렉션 객체는 다른 객체와 동등한 지위를 가진다.<br>컬렉션 객체는 반환값으로 사용할 수 있다.<br>컬렉션 객체는 필요한 경우 메서드에서 생성할 수 있다.

아래의 예시를 통해서 일급 컬렉션이 무엇이며 어떤 식으로 활용 될 수 있는지 살펴본다.

## 일급 컬렉션의 활용

아래의 요구사항을 만족하는 학점 계산기를 객체지향적으로 만들어보자.

**✅ 요구사항**

: 평균학점의 계산 방법은 (학점 수 x 교과목 평점)의 합계 / 수강신청의 총학점 수로 계산한다.<br>MVC 패턴을 기반으로 구현하도록 한다.

1. **첫번째로 학점 계산에 필요한 교과목에 관련된 객체가 필요하다.** 
   해당 객체는 학점을 계산할 수 있도록 교과목의 학점 수와 성적을 변수로 가져야 한다.

   ```java
   public class Course {
       String subject;
       int credit;
       String grade;
   
       public Course(String subject, int credit, String grade) {
           this.subject = subject;
           this.credit = credit;
           this.grade = grade;
       }
   }
   ```

2. **요구사항에 따르면 평균학점을 계산하기 위해서는 (학점 수 x 교과목 평점)의 합계가 필요하다. 합계에 관한 정보는 Course객체의 멤버 변수를 사용해서 구해낼 수 있기 때문에, Course객체에서 합계정보를 구하는 작업을 위임하여 메서드를 만든다.**

   ```java
   public class Course {
       String subject;
       int credit;
       String grade;
   
       public Course(String subject, int credit, String grade) {
           this.subject = subject;
           this.credit = credit;
           this.grade = grade;
       }
     
       public double getGradeToNumber() {
           double grade = 0;
           switch (this.grade) {
               case "A+":
                   grade = 4.5;
                   break;
               /* 이하생략 */
           }
           return grade;
       }
     
       // 학점을 계산한다.
       public double multiplyCreditAndCourseGrade(){
           return this.credit * this.getGradeToNumber();
       }
   }
   ```

3. **Course객체들을 받아 평균학점을 계산하는 GradeCalculator 객체를 만든다.**
   
   ```java
   public class GradeCalculator {
       private final List<Course> courses;
   
       public GradeCalculator(List<Course> courses) {
           this.courses = courses;
       }
       public double calculateGrade() {
           // (학점 수 x 교과목 평점)의 합계
           double multipliedCreditAndCourseGrade = 0;
           for(Course course : courses) {
               multipliedCreditAndCourseGrade += course.multiplyCreditAndCourseGrade();
           }
   
           // 수강신청 총 학점수
           int totalCompletedCredit = courses.stream()
                   .mapToInt(Course::getCredit)
                   .sum();
   
           return multipliedCreditAndCourseGrade / totalCompletedCredit;
       }
   }
   ```
   
   이 때, 학점 계산기는 여러 개의 Course 정보들을 받아서 학점을 계산하고 있다. `for(Course corsue : courses)`로 for문을 돌려 각 과목들의 총합을 구하는 로직이나 수강신청의 총 학점수를 구하는 로직 역시 사실상 Course와 관련된 로직이다. **<span style="color:red">이를 일급콜렉션을 사용하여 보다 응집도가 높게 바꿔보자.</span>**
   
4. **일급콜렉션 Courses 생성**

   ```java
   public class Courses {
       private final List<Course> courses;
   
       public Courses(List<Course> courses) {
           this.courses = courses;
       }
   }
   ```

   Course클래스를 List로 감싼 Courses 객체를 바로 일급 컬렉션이라고 할 수 있다. 이렇게 Courses를 만듦으로써 여러 과목들의 총합을 구하는 로직이나, 총 신청 학점수를 구하는 로직들을 Courses의 내부로 이동시킬 수 있다.
   
   ```java
   public class Courses {
       private final List<Course> courses;
   
       public Courses(List<Course> courses) {
           this.courses = courses;
       }
   
       // (학점 수 x 교과목 평점)의 합계
       public double multiplyCreditAndCourseGrade(){
           return courses.stream()
                         .mapToDouble(Course::multiplyCreditAndCourseGrade)
                         .sum();
       }
   
       // 수강신청 총 학점수
       public int calculateTotalCompletedCredit(){
           return courses.stream()
                         .mapToInt(Course::getCredit)
                         .sum();
       }
   }
   
   ```
   
   이렇게 로직을 이동시키면 학점계산기의 코드가 아래와 같이 깔끔하게 리팩토링 된 것을 확인할 수 있다.
   
   ```java
   public class GradeCalculator {
       private final Courses courses;
   
       public GradeCalculator(Courses courses) {
           this.courses = courses;
       }
   
       public double calculateGrade() {
           double multipliedCreditAndCourseGrade = courses.multiplyCreditAndCourseGrade();
           int totalCompletedCredit = courses.calculateTotalCompletedCredit();
   
           return multipliedCreditAndCourseGrade / totalCompletedCredit;
       }
   }
   ```

## 일급컬렉션의 장점

위와 같이 Course 클래스 를 관리하는 Courses 클래스 즉, 일급 컬렉션을 만들면 다음과 같은 장점이 있다.

- **도메인의 상태와 행위를 한 곳에서 관리할 수 있다.**

  여러 과목의 합계를 구하는 행위, 총 학점수를 구하는 행위를 Courses라는 도메인 내에서 관리하고 제어할 수 있다. 다시 말해 모든 행위를 도메인이 알고 있을 수 있기 때문에 비지니스 로직에 종속적인 자료구조를 만들 수 있다.

- **일급 컬렉션을 사용하면 Collection의 불변성을 보장할 수 있다.**

  위 예제에서 사용하진 않았지만, 일급 컬렉션은 클래스의 상태와 행위를 한 곳에서 처리하기 때문에 가변객체인 Collection을 불변으로 만들 수 있는 장점이 있다. 

[일급 컬렉션 관련 참고 링크](https://jojoldu.tistory.com/412)
