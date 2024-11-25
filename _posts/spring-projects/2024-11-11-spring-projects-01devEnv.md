---
published: true
title: "[Spring Project]개발 환경 설정"
excerpt: "개발 환경 설정하기, 웹 프로젝트 환경 구성"

categories:
  - Spring-Projects
tags: # 포스트 태그
  - [개발 환경 설정, 웹 프로젝트, gradle] 

permalink: /spring-projects/devEnv/

date: 2024-11-11
last_modified_at: 2024-11-11 # 최종 수정 날짜

---

# 웹 프로젝트 환경 구성

## build.gradle 설정

![image-20241111173330247]({{site.url}}/images/2024-11-11-spring-projects-01devEnv/image-20241111173330247.png)<br>

`implementation 'org.apache.tomcat.embed:tomcat-embed-core:9.0.79'`<br>: **tomcat-embed-core** 내장 톰캣의 코어 라이브러리

`implementation 'org.apache.tomcat.embed:tomcat-embed-jasper:9.0.79'`<br>: **tomcat-embed-jasper** JSP파일을 처리하기 위한 라이브러리, JSP를 사용하는 경우에만 추가하면 된다.

`implementation 'javax.servlet:javax.servlet-api:4.0.1'`<br>: **javax.servlet-api** 서블릿(Servlet)을 제공하는 라이브러리.

✅ 서블릿(Servlet)
 : 자바 기반의 웹 어플리케이션에서 HTTP 요청을 처리하고 동적인 웹 콘텐츠를 생성하는 자바 프로그램이다. 서버에서 실행되는 자바코드로 주로 웹 서버나 애플리케이션 서버(Tomcat, Jetty 등)에서 작동한다. 클라이언트의 요청을 받아서, 서버에서 필요한 작업을 수행한 후 동적으로 HTML 페이지, JSON 데이터, 또는 다른 응답을 생성하여 클라이언트에 반환한다.

위와 같이 build.gradle파일에 필요한 코드를 작성해주면 프로젝트 환경 설정에 필요한 라이브러리들이 자동으로 External Libraries에 추가되는 걸 확인할 수 있다. 위와 같이 필요한 라이브러리를 확인할 때에는 **<u>mavenrepository.com</u>**에서 버전을 확인한 뒤 필요한 버전을 가져와 써주면 된다.

---

#### Tomcat을 사용할 때 경로를 webapps/WEB-INF/classes 경로를 지정해주어야 하는 이유 

[톰캣 가이드](https://tomcat.apache.org/tomcat-9.0-doc/appdev/deployment.html)

위 링크에 들어가서 **Standard Directory Layout** 부분을 참고하자. 

> **/WEB-INF/classes/** <br>
> This directory contains any Java class files (and associated resources) required for your application, including both servlet and non-servlet classes, that are not combined into JAR files. If your classes are organized into Java packages, you must reflect this in the directory hierarchy under `/WEB-INF/classes/`. <br>For example, a Java class named `com.mycompany.mypackage.MyServlet` would need to be stored in a file named `/WEB-INF/classes/com/mycompany/mypackage/MyServlet.class`. 