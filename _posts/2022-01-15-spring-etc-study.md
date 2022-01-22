---
layout: post
title:  "spring 관련 자주 참고하는 정보등.."
date:   2022-01-15 11:00
categories: archives
tags: spring study
---

## redirect vs forward 
redirect는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. <br>
따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. (2번 호출..)<br><br>
반면에 forward는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.

## mvc 패턴 - 한계
공통 처리가 어렵다. <br>
컨트롤러 호출 전에 먼저 공통기능을 처리해야 한다. 
Front Controller Pattern을 도입하면 이런 문제를 해결할 수 있다. (수문장 역할 - 입구를 하나로!) <br>
필터는 정해진 스펙대로 filter 를 넘기는 것일뿐..

## front-controller pattern 특징
- 서블릿 하나로 클라이언트 요청을 받음 
- 입구를 하나로, 공통 처리 가능 
- 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨
- 스프링 웹 mvc 핵심도 front-controller이며, dispatcherServlet 이 front-controller pattern으로 구현되어 있음

## 다형성 (polymorphism)
하나의 객체가 여러 가지 타입을 가질 수 있는 것을 의미한다.
자바에서는 이러한 다형성을 부모 클래스 타입의 참조 변수로 자식 클래스 타입의 인스턴스를 참조할 수 있도록 구현하고 있다.

## adapter pattern
- 한 클래스의 인터페이스를 클라이언트에서 사용하고자하는 다른 인터페이스로 변환한다. (ex: 110v를 220v로 변환)
- 어댑터를 이용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다. 

## HandlerMapping 우선 순위 
0. RequestMappingHandlerMapping : 애노테이션 기반의 컨트롤러의 @RequestMapping에서 사용
1. BeanNameUrlHandlerMapping : 스프링 빈의 이름으로 핸들러를 찾는다.

## HandlerAdapter 우선 순위
0. HandlerMapping 후 전달 받은 Handler가 존재해야함
1. RequestMappingHandlerAdapter : 애노테이션 기반의 컨트롤러의 @RequestMapping에서 사용
2. HttpRequestHandlerAdapter : HttpRequestFilter 처리
3. SimpleControllerHandlerAdapter : Controller 인터페이스(애너테이션x, 과거에 사용) 처리

## spring boot 기본 제공 로깅
- 인터페이스 slf4j, 구현체 logback
- spring-starter-logging 내 존재
```java
// +가 아닌 format 으로 사용해야 연산이 발생하지 않는다.
// log.trace("test"+ "1"); // 노출되지 않아도 연산됨
// log.trace("trace{}", 1); // 연산 발생하지 않음
```
## consume && produces
- server 입장에서 생각하면 됨
- consume : server 가 소비할 형식 (client 기준 Content-Type)
- produces : server 가 생산할 형식 (client 기준 Accept)

## spring boot default message-converter
- 대상 클래스 타입과 미디어 타입 둘을 체크해서 사용여부를 결정한다. 아래 우선순위에 해당하지 않으면 탈락!
- 요청 시점 ArgumentResolver && 응답 시점 ReturnValueHandler에서 호출됨
0. ByteArrayHttpMessageConverter
```java
// 클래스 타입: byte[] , 미디어타입: */*
// 요청 예) @RequestBody byte[] data
// 응답 예) @ResponseBody return byte[] 쓰기 미디어타입 application/octet-stream
```
2. StringHttpMessageConverter
```java
//클래스 타입: String , 미디어타입: */*
//요청 예) @RequestBody String data
//응답 예) @ResponseBody return "ok" 쓰기 미디어타입 text/plain
```
3. MappingJackson2HttpMessageConverter
```java
//클래스 타입: 객체 또는 HashMap , 미디어타입 application/json 관련
//요청 예) @RequestBody HelloData data
//응답 예) @ResponseBody return helloData 쓰기 미디어타입 application/json 관련
```



## 효율적인 구조 개선 (리팩토링) 방안
1. 우선 구조를 바꾼다.
2. 그 다음에 세세한것을 개선한다.
3. 한번에 하면 실수가 생길 여지가 있다..
4. 아무리 아키텍쳐가 좋아도 실사용자가 단순하고 편리하게 사용할 수 있어야 한다.
5. 프레임워크나 공통 기능이 수고로워야 사용하는 개발자가 편리해진다.



## 참고
1. 인프런 김영한님 스프링 MVC 강의
2. [어댑터 패턴](https://jusungpark.tistory.com/22)
3. [@Controller 사용 가능한 파라메터 목록](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments)


