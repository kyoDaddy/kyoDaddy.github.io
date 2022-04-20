---
layout: post
title:  "파이썬 도커 이미지 빌드"
date:   2022-04-05 21:56
categories: archives
tags: python 
---


## 파이썬 도커 이미지 빌드
1. 패키지 버전 선언 : requirement.txt
```text
mysqlclient==2.1.0
uWSGI==2.0.20
uwsgitop==0.11
...
```

2. 도커 이미지 : Dockerfile (원하는 버전을 찾아서 파일 작성한다...왜 늘 새로운 거죠..?)

~~~dockerfile
FROM python:3.9.1 AS builder

RUN apt-get update \
	&& apt-get -y --no-install-recommends install libsasl2-dev \
	&& mkdir /apps

ADD requirements.txt /apps
WORKDIR /apps

RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install pyinstrument

FROM python:3.9.1-slim AS runner
RUN apt update \
    && apt install -y libxml2 \
    && apt install -y default-libmysqlclient-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/
COPY --from=builder /usr/local/bin/uwsgi /usr/local/bin/uwsgi
#COPY start.sh /apps/start.sh

ADD . /apps
ENV PYTHONUNBUFFERED=0
WORKDIR /apps

RUN rm -rf static
RUN mkdir static
RUN python3 manage.py collectstatic --noinput

CMD ["python",  "manage.py", "runserver", "0.0.0.0:8000"]
EXPOSE 8000
~~~

3. 도커 이미지 빌드
```shell
docker build -t python-build-test .
```



