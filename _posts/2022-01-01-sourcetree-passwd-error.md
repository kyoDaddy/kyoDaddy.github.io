---
layout: post
title:  "sourcetree authentication failed 발생시"
date:   2022-01-01 22:00
categories: archives
tags: tool solution
---
## Athentication failed error 발생시

git-karaken과 sourcetree를 둘다 사용하고 있는데,
그중에 주기적으로 gitlab 비밀번호 변경시에마다 sourcretree에서 "sourcetree authentication failed error" 가 발생하였다.

소스트리가 비밀번호를 저장해두는 파일을 삭제하여 문제를 해결했다.
(기존에 북마크해서 참고하던 블로그가 폭발해서;; 기록으로 남겨둠)

## source tree passwd 파일 삭제
```
DEL C:\Users\[사용자계정]\AppData\Local\Atlassian\SourceTree\passwd
```

## 참고 레퍼런스
   1. [Sourcetree 에서 잘못된 비밀번호로 저장소 접근 안될때 해결방법](https://shanepark.tistory.com/214)
