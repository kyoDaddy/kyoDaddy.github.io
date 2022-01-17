---
layout: post
title:  "spring 자주 까먹는 기타 정보들.."
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



## 참고
1. 인프런 김영한님 스프링 MVC 강의


