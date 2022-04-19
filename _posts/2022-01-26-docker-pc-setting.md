---
layout: post
title:  "docker 개인pc에서 자주 사용하는 명령어 모음"
date:   2022-01-26 12:33
categories: archives
tags: docker mariadb
---

## 도커 이미지 받아와 컨테이너 올리기 (mariadb)
```shell
docker pull mariadb
docker run --name mariadb -d -p 3306:3306 --restart=always -e MYSQL_ROOT_PASSWORD=mariadb mariadb
```

## 도커 컨테이너 접근 (mariadb)
```shell
docker exec -it mariadb /bin/bash
mysql -u root -p
use dbName;
grant all privillages on *.* to 'root'@'%' identified by 'test!1234';
flush privillages;
```

## 도커 이미지 생성 및 컨테이너 실행/접근 (redis)
```shell
docker pull redis
# docker network 구성(필요시)
docker network create redis-net
# 실행
docker run --name kyo-redis -p 6379:6379 --network redis-net -d redis redis-server --appendonly yes
# 접근
docker exec -it kyo-redis /bin/bash

# redis-cli 접속
#redis-cli -h 호스트면 -p 포트번호
# redis-cli 정보보기
redis-cli info
# redis-cli crud 명령어
set key_one value_one
mset key_two value_two key_three value_three
setex key_four 10 key_four

get key_four
mget key_two key_three 

del key_three

# redis-cli 키 재정의
rename kyo_one key_one

# redis-cli 모든 데이터 삭제
flushall

# redis-cli 카운터 생성
set test_cnt
incr test_cnt
get test_cnt

```

## 도커 이미지 빌드 및 실행
~~~shell
# 도커 이미지 빌드 (도커파일명으로 찾아서 빌드)
$ docker build -f Dockerfile -t build-docker .

# 만든 이미지로 도커 실행 (환경변수 전달)
docker run -e RDS_CREDENTIAL={\”password\”:\”aa\”bb\”} -e RUN_ENV=dev --name test
~~~



## 참고
1. [Docker - 도커로 Mariadb 컨테이너 간편하게 설치하기](https://7942yongdae.tistory.com/130)
2. [redis-cli 명령어 정리](https://freeblogger.tistory.com/10)

