---
layout: post
title:  "docker 개인pc에서 자주 사용하는 명령어 모음"
date:   2022-01-26 12:33
categories: archives
tags: docker mariadb
---

## 도커 이미지 받아와 컨테이너 올리기
```shell
docker pull mariadb
docker run --name mariadb -d -p 3306:3306 --restart=always -e MYSQL_ROOT_PASSWORD=mariadb mariadb
```

## 도커 컨테이너 접근
```shell
docker exec -it mariadb /bin/bash
mysql -u root -p
use dbName;
grant all privillages on *.* to 'root'@'%' identified by 'test!1234';
flush privillages;
```


## 참고
1. [Docker - 도커로 Mariadb 컨테이너 간편하게 설치하기](https://7942yongdae.tistory.com/130)

