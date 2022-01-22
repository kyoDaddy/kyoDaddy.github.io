---
layout: post
title:  "리눅스 쉘스크립트 & 크론 사용법"
date:   2022-01-21 07:48
categories: archives
tags: linux shell cron
---

프로젝트로 띄울 정도는 아니었어서, 아주 예전 기억을 찾아서 기록한다.
1. jar 실행 및 로깅파일 생성 관련 shell script 작성
2. sample-batch-process.jar 배포 
3. cron 등록하여 주기적으로 sample-batch-process.jar 실행

## cron
리눅스/유닉스 크론 표현식 (초부터 시작하는 스프링 크론 표현식과는 다르게 분부터 시작)
```
30 0 * * * sudo /sample/scripts/batch-process.sh start > /sample/log/sample-batch-process/cron.log.$(date +\%Y-\%m-\%d) 2>&1
```

## shell

```shell
#!/usr/bin/env bash
# group batchProcess dev

# Configurations
JAVA_HOME=/sample/jdk

BATCHPROCESS_HOME=/sample/app/sample-batch-process/
LOG_DIR=/sample/log/sample-batch-process/
LOG_LEVEL=DEBUG
PROCESS_ENV=dev
SENTINEL_HOSTNAME=127.0.0.1
SENTINEL_PORT=port
REDIS_PASSWORD=password
MAX_THREAD=200

if [ -n "$2" ]
then
        LOG_LEVEL="$2"
fi

# Generate VM Options
JAVA_OPTIONS+=("-Dprocess.Env=$PROCESS_ENV")
JAVA_OPTIONS+=(" -Dlog.dir=$LOG_DIR")
JAVA_OPTIONS+=(" -Dlog.level=$LOG_LEVEL")
JAVA_OPTIONS+=(" -Dredis_sentinel.password=$REDIS_PASSWORD")
JAVA_OPTIONS+=(" -Dredis_sentinel.port=$SENTINEL_PORT")
JAVA_OPTIONS+=(" -Dredis_sentinel.hostname=$SENTINEL_HOSTNAME")
JAVA_OPTIONS+=(" -Dmax_thread=$MAX_THREAD")

JAVA_OPTIONS=${JAVA_OPTIONS[@]}

if [ -w $BATCHPROCESS_HOME ]
then
        if [ "$1" = "start" ]
        then
                if [ -e $BATCHPROCESS_HOME/pid ]
                then
                        echo "Sample Batch Process already Started"
                else
                        nohup $JAVA_HOME/bin/java $JAVA_OPTIONS -jar $BATCHPROCESS_HOME/jar/sample-batch-process.jar > ${LOG_DIR}/nohup.log.$(date +\%Y-\%m-\%d) 2>&1 &
                        echo $! > "$BATCHPROCESS_HOME/pid"
                        cmd=$(cat $BATCHPROCESS_HOME/pid)
                        echo "Sample Batch Process start complete. pid:$cmd"
                        sleep 10
                        rm $BATCHPROCESS_HOME/pid
                fi
        elif [ "$1" = "stop" ]
        then
                if [ -f $BATCHPROCESS_HOME/pid ]
                then
                        cmd=$(cat $BATCHPROCESS_HOME/pid)
                        rm $BATCHPROCESS_HOME/pid
                        echo "Sample Batch Process stop complete"
                fi
        else
                echo "input Parameter error"
                echo "Usage : batchProcess star|stop"
        fi

else
        echo "Permission Required!"
fi

```


## 참고
1. [쉘 스크립트 기본 문법](https://reakwon.tistory.com/136)
2. [크론 표현식](https://gs.saro.me/dev?tn=548)

