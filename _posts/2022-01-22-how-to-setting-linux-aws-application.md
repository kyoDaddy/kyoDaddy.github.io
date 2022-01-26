---
layout: post
title:  "리눅스 쉘스크립트 & 크론 사용법 (2) - 개발 인스턴스 s3 소스 동기화 구성"
date:   2022-01-22 22:33
categories: archives
tags: linux aws deploy shell cron
---
git을 통해 s3에 업로드 후에, 
개발 인스턴스에 가져와 실행시키는 부분을 샘플링 해서 남겨둔다.
다음번에는 codedeploy를 사용하면.. 개발 쪽 배포 구성을 좀 더 심플하게 할 수 있지 않을까??

## Update Package Manager
```text
$ sudo yum -y update
```

## Install Java (openjdk-11)
```text
$ sudo amazon-linux-extras install java-openjdk11
```

## AWS Profile 설정 
- 관리형 S3 에서 파일을 가져오기 위해서 설정 필요
- default profile 설정
- 필요시 별도 profile 설정 필요
 
```text
$ sudo aws configure set default.aws_access_key_id AWS-Access-Key-ID
$ sudo aws configure set default.aws_secret_access_key AWS-Secret-Access-Key
$ sudo aws configure set default.region ap-northeast-2
$ sudo aws configure set default.output json
```  

## 디렉토리 생성 및 심볼릭 설정
- /sample : 운영을 위한 기본 폴더
- /sample/app : application 소스 파일 위치
- /sample/log : application 로그 파일 위치
- /sample/script : 운영을 위해 필요한 스크립트들

```text
$ sudo mkdir /sample
$ cd /sample
$ sudo ln -s /etc/alternatives/jre_11 jre

$ sudo mkdir -p /etc/default/sample
$ sudo ln -s /etc/default/sample env

$ sudo mkdir -p /srv/www/sample
$ sudo ln -s /srv/www/sample app

$ sudo mkdir -p /var/log/sample
$ sudo ln -s /var/log/sample log
```

## aws 업로드한 환경변수 파일 다운로드
```text
$ sudo aws s3 cp s3://{test|live}-sample/ops-scripts/env/sample-api/{test|live}-sample-api-env /etc/default/sample/sample-api

$ cat /etc/default/sample/sample-api
env=dev
profile=default
port=1111
bucket_name=a
version=latest
jvm_opts="-Dfile.encoding=UTF-8 -Duser.timezone=GMT+09:00 -Denv=dev -Dspring.profiles.active=dev -Xms300m -Xmx300m -server"
```

## boot 스크립트 init.d 등록
```text
$ sudo aws s3 cp s3://{test|live}-sample/ops-scripts/java/sample-api/sample-api.sh /sample/script/sample-api.sh
$ sudo sed -i -e 's/\r$//' /sample/script/sample-api.sh

$ sudo chkconfig --del sample-api
$ sudo cp /sample/script/sample-api.sh /etc/init.d/sample-api
$ sudo chmod +x /etc/init.d/sample-api
$ sudo chkconfig --add sample-api

$ cat /sample/script/sample-api.sh
#!/bin/bash
#
# chkconfig: 345 96 30
# description: Start up spring-boot server.

command=$1
app_group=sample
script_file_name="${0##*/}"
app_name="${script_file_name%.*}"

app_path=/${app_group}/app
env_file=/${app_group}/env/${app_name}

s3_recently_path=$3

# load env
sudo touch ${env_file}
. ${env_file}
export env
export port
export profile
export bucket_name
export version
export jvm_opts

# parameter
while [ "$2" != "" ]; do
  case $2 in
    -env) shift;
      env=$2
      ;;
    -port) shift;
      port=$2
      ;;
    -profile) shift;
      profile=$2
      ;;
    -bucket_name) shift;
      bucket_name=$2
      ;;
    -version) shift;
      version=$2
      ;;
    -jvm_opts) shift;
      jvm_opts=$2
      ;;
    *) exit 1
  esac
  shift
done

pid=$(ps -ef | grep ${app_name}.jar | egrep -v 'grep|rsync' | awk '{print $2} ' | sed 's/[^0-9]//g')

start() {
  echo "[START] start"
  check_install
  if is_running; then
    echo "[START] ${app_name} (pid ${pid}) is already running"
    return 0
  fi

  echo "[START] step 1 : start ${app_name} ..."
  pushd ${app_path}/${app_name} > /dev/null

  aws_access_id="$(aws configure get profile.${profile}.aws_access_key_id)"
  aws_secret_key="$(aws configure get profile.${profile}.aws_secret_access_key)"

  ###java -Dcloud.aws.credentials.accessKey=${aws_access_id} -Dcloud.aws.credentials.secretKey=${aws_secret_key} ${jvm_opts} -jar ${app_name}.jar
  nohup java -Dcloud.aws.credentials.accessKey=${aws_access_id} -Dcloud.aws.credentials.secretKey=${aws_secret_key} ${jvm_opts} -jar ${app_name}.jar > /dev/null 2>&1 &

  # if [ "${env}" = "dev" ]; then
  #   echo "[START] step 2 : waiting 5 seconds ..."
  #   sleep 5
  # else
  #   echo "[START] step 2 : waiting 30 seconds ..."
  #   sleep 30
  # fi

  popd > /dev/null

  echo "[START] ${app_name} has started"
}

stop() {
  echo "[STOP] start"
  check_install

  if is_running; then
    pushd ${app_path} > /dev/null

    # echo "[STOP] step 1 : close the ELB ..."
    # down

    # if [ "${env}" = "dev" ]; then
    #   echo "[STOP] step 1 : waiting 5 seconds ..."
    #   sleep 5
    # else
    #   echo "[STOP] step 1 : waiting 30 seconds ..."
    #   sleep 30
    # fi

    echo "[STOP] step 1 : kill the ${app_name} ..."
    kill -15 ${pid}
    while is_running;
    do
      echo "[STOP] step 2 : ${app_name} (pid ${pid}) is running..."
      sleep 5
    done

    popd > /dev/null
  fi

  echo "[STOP] ${app_name} has stopped"
}

status() {
  echo "[STATUS] start"
  echo "[STATUS] ${app_name} params"
  echo "env - $env"
  echo "port - $port"
  echo "profile - $profile"
  echo "bucket_name - $bucket_name"
  echo "version - $version"
  echo "jvm_opts - $jvm_opts"

  if is_running; then
    if is_down; then
      echo "[STATUS] ${app_name} (pid ${pid}) is running and down"
    else
      echo "[STATUS] ${app_name} (pid ${pid}) is running"
    fi
  else
    echo "[STATUS] ${app_name} is either stopped or inaccessible"
  fi
}

# functions
usage_install() {
  echo "Usage: ${app_name}.sh install {options}"
  echo "[Options]"
  echo "-env {dev|test|live}"
  echo "-port {service port}"
  echo "-profile {aws credential profile}"
  echo "-bucket_name {aws s3 bucket name}"
  echo "-version {build version}"
  echo "-jvm_opts {jvm_opts}"
}

usage_service() {
  echo "Usage: service ${app_name} {commands}"
  echo "[Commands]"
  echo "- start"
  echo "- stop"
  echo "- status"
  echo "- restart"
}

is_running() {
  pid=$(ps -ef | grep ${app_name}.jar | egrep -v 'grep|rsync' | awk '{print $2} ' | sed 's/[^0-9]//g')

  if echo "${pid}" | egrep -q '^\-?[0-9]*\.?[0-9]+$'; then
    return 0
  else
    return 1
  fi
}

check_permission() {
  if ! [ -w ${env_file} ]; then
    echo "permission required!"
    exit 2
  fi
}

check_install() {
  if ! [ -w ${app_path} ]; then
    usage_install
    exit 1
  fi
}

is_down() {
  local http_status="$(curl -o /dev/null -s -w '%{http_code}\n' localhost:${port}/manage/health)"
  if [ "${http_status}" = "200" ]; then
    return 1
  else
    return 0
  fi
}

# main
check_permission
case "${command}" in
  start) start ;;
  stop) stop ;;
  restart) stop && start ;;
  status) status ;;
  *) usage_service ;;
esac
You have mail in /var/spool/mail/root
```

## 최신소스 업데이트
```text
$ sudo aws s3 cp s3://{test|live}-sample/ops-scripts/java/sample-api/s3-sync-sample-api.sh /sample/script/s3-sync-sample-api.sh
$ sudo chmod +x /sample/script/s3-sync-sample-api.sh
$ sudo sed -i -e 's/\r$//' /sample/script/s3-sync-sample-api.sh
$ sudo sh /sample/script/s3-sync-sample-api.sh s3://{test|live}-sample/sample/sample-api/ /sample/app/sample-api/

$ cat /sample/script/s3-sync-sample-api.sh
#!/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

origin_path=$1
app_path=$2

sync_file_count=$(($(aws s3 sync ${origin_path} ${app_path} --dryrun | grep download: | wc -l) + 0))

if [[ $sync_file_count>0 ]]; then
  aws s3 cp ${origin_path} ${app_path} --recursive
  service sample-api restart
  echo "Done."
else
  echo "Already up to date."
fi
```

## cron
```text
* * * * * sh /sample/script/s3-sync-sample-api.sh s3://deploy/sample/sample-api/ /sample/app/sample-api/
```

## 실행 명령
```text
$ sudo service sample-api stop
$ sudo service sample-api start
$ sudo service sample-api restart
$ sudo service sample-api status
```

## Application AutoScale 대상 Deploy 스크립트 추가
```text
$ sudo aws s3 cp s3://{test|live}-sample/sample/sample-api/recently/sample-api-recently.jar /sample/app/sample-api/sample-api.jar --profile=default
$ sudo service sample-api restart
```
    

## 참고
1. [ec2에 s3 버킷 접근 권한 부여](https://cjsal95.tistory.com/28)
2. [spring + github-actions + code-deploy 1,2편 다보기](https://devlog-wjdrbs96.tistory.com/361)
2. [springboot project aws ec2 instance deploy](https://victorydntmd.tistory.com/338)

