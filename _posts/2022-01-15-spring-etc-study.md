---
layout: post
title:  "spring 관련 자주 참고하는 정보 모음"
date:   2022-01-15 11:00
categories: archives
tags: log4j log4j2 solution
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
- 한 클래스의 인터페이스를 클라이언트에서 사용하고자하는 다른 인터페이스로 변환한다.
- 어댑터를 이용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다.



## 효율적인 구조 개선 (리팩토링) 방안
1. 우선 구조를 바꾼다.
2. 그 다음에 세세한것을 개선한다.
3. 한번에 하면 실수가 생길 여지가 있다..
4. 아무리 아키텍쳐가 좋아도 실사용자가 단순하고 편리하게 사용할 수 있어야 한다.



## 참고
1. 인프런 김영한님 스프링 MVC 강의
2. [어댑터 패턴](https://jusungpark.tistory.com/22)


