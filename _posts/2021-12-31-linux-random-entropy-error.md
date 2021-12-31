---
layout: post
title:  "Too many open files error 발생시"
date:   2021-12-31 10:00
categories: archives
tags: linux aws
---
## "Too many open files" error 발생시

aws ec2 운영시 갑자기 "Too many open files" 발생 후 어플리케션이 죽어버릴 경우가 생겼다.

프로세스가 OS에 요청할 수있는 리소스의 개수에 limit가 있고, 프로세스가 그 제한을 넘었기 때문에 발생한 것이었다.
해결책은 계정과, 프로세스 모두 limit를 늘려주니 같은 문제가 발생하지 않았다.

## 계정 NOFILE limit 변경
1. 프로세스가 실행되는 계정의 file default limit
```bash
$ ulimit  -Hn
4096
$ ulimit -Sn
1024
```

2. root 계정으로 /etc/security/limits.conf에 다음 내용을 추가 (system 전체 limit 보다 작아야 함..)
```bash
* hard nofile 500000
* soft nofile 500000
root hard nofile 500000
root soft nofile 500000
```
  
3. system 전체 limit 확인 
```bash
$ cat /proc/sys/fs/file-max
```

4. 계정에 할당된 limit 변경했다면, 터미널 로그아웃 후 재로그인 하여 ulimit 값이 변경되어 있는지 확인


## 프로세스 NOFILE limit 변경
1. 프로세스 PID 확인
```bash
$ ps -ef | grep [프로세스명]
2321
```

2. 해당 프로세스의 NOFILE limit 확인
```bash
$ prlimit --nofile --output RESOURCE,SOFT,HARD --pid 2321
```

3. 프로세스 limit 변경
```bash
$ prlimit --nofile=500000 --pid=2321
```

## 기타
1. 튜닝은 꼭 관련 문서를 읽어보고 진행해보자...
2. 참고 레퍼런스
   1. [Increase “Open Files Limit”](https://aws-labs.com/increase-open-files-limit/)
   2. [Too many open files 에러, 소켓 제한 늘리기](https://devbrain.tistory.com/53)
   3. [Java, max user processes, open files](https://techblog.woowahan.com/2569/)
