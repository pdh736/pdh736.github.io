---
title: "config"
excerpt: "How to set git config"

categories:
  - git

tags:
  - config
  - setting

toc: true
toc_sticky: true

date: 2020-11-11
last_modified_at: 2020-11-11
---
# git config

## 설정
```
git config --global user.name "user_name"

git config --global user.email "user@email.com"
```


## 조회
```
git config --list

git config --list --global
```


## 삭제
```
git config --unset user.name

//global 설정 지울경우
git config --unset --global user.name
```

## Alias

Git의 유용한 방법 중 하나인 Alias의 추가, 삭제 및 목록 명령어

#### Alias 추가
```
//전역에 Alias 추가
$ git config --global alias.ci commit

//지역에 Alias 추가
$ git config alias.ci commit
```

#### Alias 삭제
```
//전역 Alias 삭제
$ git config --global --unset alias.ci

//지역 Alias 삭제
$ git config --unset alias.ci
```

#### Alias 조회

```
//전역 Alias 조회
$ git config --global --get-regexp alias

//지역 Alias 조회
$ git config --local --get-regexp alias

//전역 및 지역 Alias 조회
$ git config --get-regexp alias	
```

## .gitconfig 파일 내용
$HOME/.gitconfig
```
[user]
    email = xxxx@xxxx.com
    name = Daehee.Park
[core]
    editor = vim 
    autocrlf = input
    autiocrlf = input
    whitespace = " trailing-space,space-before-tab,cr-at-eol"
[push]
    default = matching
[alias]
    hide = update-index --assume-unchanged
    unhide = update-index --no-assume-unchanged
    d = difftool
[diff]
    tool = vimdiff
[difftool]
    prompt = false
```

