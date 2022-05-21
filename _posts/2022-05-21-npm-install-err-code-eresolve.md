---
layout: post
title:  "npm install 시 code ERESOLVE 발생시.."
date:   2022-05-21 11:40
categories: archives
tags: react npm
---

## 현상 
react > pacakage.json 위치에서 npm install 시 code ERESOLVE 에러 발생
~~~
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! 
npm ERR! While resolving: materialize-mui-react-nextjs-admin-template@1.0.0
npm ERR! Found: react@18.1.0
npm ERR! node_modules/react
npm ERR!   react@"18.1.0" from the root project
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer react@"^15.0.0 || ^16.0.0 || ^17.0.0" from @casl/react@2.3.0
npm ERR! node_modules/@casl/react
npm ERR!   @casl/react@"2.3.0" from the root project
npm ERR! 
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force, or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR! 
~~~

## 해결
~~~shell
npm install --save --legacy-peer-deps
npm audit fix --force
~~~


## 참고
1. [레퍼런스1](https://iancoding.tistory.com/154)
