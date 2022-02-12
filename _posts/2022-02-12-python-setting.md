---
layout: post
title:  "python 로컬 설치 등.."
date:   2022-02-12 22:31
categories: archives
tags: python
---


## mac (m1)
### python (3.x) 설치
```shell
$ brew install python
$ python3 -V
$ which python3
$ /opt/homebrew/bin/python3
```

### 파이썬3 바라보도록 변경 (only python 3.x)
```shell
$ vi ~/.zshrc
# 맨아래에 추가
$ # Only Python3
$ alias python="/opt/homebrew/bin/python3"
$ alias pip="/opt/homebrew/bin/pip3"
$ wq!
$ source ~/.zshrc
$ which python
$ /opt/homebrew/bin/python3
```

### pyenv, virtualenv로 여러 파이썬 버전 변경하기
```shell
$ brew install pyenv pyenv-virtualenv

$ vi ~/.zshrc
# 맨아래에 추가
# stting pyenv
$ export PYENV_ROOT="$HOME/.pyenv"
$ export PATH="$PYENV_ROOT/bin:$PATH"
$ eval "$(pyenv init --path)"
$ eval "$(pyenv init -)"
$ eval "$(pyenv virtualenv-init -)"
$ wq!
$ source ~/.zshrc


# 설치 가능한 Python 버전
$ pyenv install --list
# 특정한 버전 Python 설치
$ pyenv install 3.9.10
# 특정한 버전 Python 삭제
$ pyenv uninstall 3.9.10
# 설치된 Python list
$ pyenv versions
# 해당 Python 버전을 기본으로 설정
$ pyenv global 3.9.10


#가상환경 만들기 - pyenv virtualenv [version] [name]
$ pyenv virtualenv 3.9.10 py39
# 가상환경 시작하기
$ pyenv activate py39
# 가상환경 종료하기
$ pyenv deactivate
# 가상환경 목룍보기
$ pyenv virtualenvs
```


## 참고
1. [M1 python3 설치](https://imksh.com/90)
2. [mac python(pyenv) 설치 및 버전관리](https://leesh90.github.io/environment/2021/04/03/python-install/)
3. [여러 버전의 파이썬 관리하기 (pyenv)](https://www.daleseo.com/python-pyenv/)