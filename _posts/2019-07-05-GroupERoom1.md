---
layout: post
title: 'GroupERoom 로그_1 (스프링 환경세팅, maven, GitHub)'
subtitle: 'Spring 개인 프로젝트'
date: 2019-07-05
categories: Spring
author: JY
cover: 'https://user-images.githubusercontent.com/38846776/60693071-c3390600-9f13-11e9-85e6-cbba76cb5159.png'
tags: Spring, Maven, GitHub
---

# Spring 환경셋팅

## 190705 
1. 프로젝트 시작 전, 필요한 셋팅들
- 자바 JDK, Eclipse 설치
- 환경 변수 설정
- Eclipse MarketPlace에서 STS 설치

2. 이클립스에서 새로운 프로젝트 생성
- New -> Other -> Spring Legacy Project
![0705_1](https://user-images.githubusercontent.com/38846776/60693122-f7acc200-9f13-11e9-805e-01da4fae6f8f.PNG)

- 프로젝트 명과 Spring MVC Project 선택 후 NEXT
![0705_2](https://user-images.githubusercontent.com/38846776/60693181-36427c80-9f14-11e9-8163-f9b12534481a.PNG)

- 프로젝트 셋팅 - 패키지 이름 정의
패키지 세번째 이름이 context root가 된다. 
*저기서는 ger이 context root*
![0705_3](https://user-images.githubusercontent.com/38846776/60693238-84578000-9f14-11e9-9cf2-ca99c9fa0b4c.PNG)

- 생성 후, 초기 구조
![0705_4](https://user-images.githubusercontent.com/38846776/60693253-920d0580-9f14-11e9-80b4-bf44d0080897.PNG)

3. 프로젝트 생성하면서 problems 탭 켜놓고 에러 확인
메이븐 update project 한번 해주기

4. Tomcat 실행해보기 (v8.5.35 사용) 
- console에 오류가 찍힌다.
![0705_5](https://user-images.githubusercontent.com/38846776/60694051-b4ece900-9f17-11e9-81c8-59f684361eb5.PNG)
읽어보니
 
```
[The absolute uri: [http://java.sun.com/jsp/jstl/core] cannot be resolved
```

라는 오류가 눈에 띈다. 

- jstl jar 파일을 프로젝트에 넣어준다.
![0705_6](https://user-images.githubusercontent.com/38846776/60694052-b61e1600-9f17-11e9-9701-ba30555050c6.PNG)
폴더구조는 WEB-INF아래에 lib 폴더를 생성하여 jstl-1.2.jar파일을 넣어주었다.

- 다시 돌려보기 전에 처음으로 보여지는 home.jsp를 약간 수정했다.
*+) home.jsp는 WEB-INF/views에 위치해 있다.*

- 다시 톰캣을 실행했을 때, 정상적으로 home 페이지가 나오는 것을 확인.
![0705_7](https://user-images.githubusercontent.com/38846776/60700466-fccd3980-9f32-11e9-8ba1-5ce0494d4d94.PNG)
콘솔에서는 HomeController에서 적혀진 로그에 따라 다음과 같이 출력된다.

```
INFO : com.jy.ger.HomeController - Welcome home! The client locale is ko_KR.
```

5. pom.xml의 설정 변경
<properties>태그의 내용을 다음과 같이 변경
	
```
<properties>
	<java-version>1.8</java-version>
	<org.springframework-version>5.0.8.RELEASE</org.springframework-version>
	<org.aspectj-version>1.8.13</org.aspectj-version>
	<org.slf4j-version>1.7.25</org.slf4j-version>
</properties>
```

6. project faects 변경
프로젝트 alt + Enter를 통해 properties의 project facets를 변경한다.
- JAVA의 버전을 1.8로
- Runtimes 탭의 서버에 체크

![0705_8](https://user-images.githubusercontent.com/38846776/60700888-77e31f80-9f34-11e9-9056-a5358ee2d8cf.PNG)

7. maven 설정
- 메이븐 다운로드(v3.6.0)
- 메이픈 폴더 안에 repository 폴더 생성
 ![0705_9](https://user-images.githubusercontent.com/38846776/60701418-ac57db00-9f36-11e9-972c-867c93df9c85.PNG)
*생성한 respository까지의 경로를 복사해놓기!*
- 메이븐 config폴더 안의 settings.xml파일 워드패드/노트패드로 열기
<localRepository></localRepository> 태그를 검색을 통해 찾아서 주석을 해제하고
태그안에 경로 붙여넣고 저장

```
<localRepository>D:/JY/DEV/apache-maven-3.6.0/respository</localRepository>
```

- 이클립스에서 window > preference > maven > userSettings 으로 이동

![0705_10](https://user-images.githubusercontent.com/38846776/60702082-cc889980-9f38-11e9-97b0-1912c31b7a70.PNG)

borwse 버튼을 통해 경로를 방금 설정한 settings.xml까지 잡아준다.
*메이븐 업데이트까지 해주자*

8. 폴더 변경
- WEB-INF/spring 폴더명을 config로 수정
- confing/appServlet/servlet-context.xml을 config 아래로 이동
- servlet-context.xml -> action-servlet.xml으로 변경
- root-context.xml을 src/main/resources아래로 이동

![0705_11](https://user-images.githubusercontent.com/38846776/60702947-52a5df80-9f3b-11e9-8795-cf9060c0d1ad.PNG)

9. web.xml

```
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:root-context.xml</param-value>
</context-param>

<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<servlet>
	<servlet-name>action</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/config/action-servlet.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
```


10. <listener> 태그 주석 처리하고 webapp 아래에 index.jsp 생성하고 서버 실행
--> 성공적으로 인덱스 페이지 로드

![0705_12](https://user-images.githubusercontent.com/38846776/60703929-fa241180-9f3d-11e9-895d-cbd65f0a37ce.PNG)




