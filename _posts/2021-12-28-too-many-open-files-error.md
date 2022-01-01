---
layout: post
title:  "linux random entropy 부족시"
date:   2021-12-28 10:00
categories: archives
tags: linux openstack solution
---
## linux random entropy 부족시
오픈스택에서 톰캣을 실행시키는데 한참이 걸려서, 이유가 무엇인지 찾다가 stackoverflow에서 찾았다...
사유는 엔트로피가 부족해서이며, haveged 사용하여 해결했다.

## 엔트로피 가용량 확인
```bash
cat /proc/sys/kernel/random/entropy_avail
27
```

## haveged
Hardware Volatile Entropy Gathering and Expansion : 하드웨어 휘발성 엔트로피 수집 및 확장
```bash
$ sudo yum -y install haveged
$ haveged
```


## 기타
1. 참고 레퍼런스
   1. [How to use GnuPG inside Docker containers, as it is missing entropy?](https://stackoverflow.com/questions/30573896/how-to-use-gnupg-inside-docker-containers-as-it-is-missing-entropy)
   2. [How To Uninstall haveged on CentOS 7](https://installati.one/centos/7/haveged/)
