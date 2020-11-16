---
title: "rebase"
excerpt: "How to set git rebase"

categories:
  - git
tags:
  - git
  - reset

toc: true
toc_sticky: true

date: 2020-11-16
last_modified_at: 2020-11-16
---

# git rebase
```
# git rebase [name]  
```
HEAD가 가르킨 브랜치를 name(커밋 or 브랜치) 뒤로 복사 생성  
fast forward 일경우 그냥 이동만 함  

```
# git rebase master
//현재 head부터  master가 포함하지 않은 커밋까지 master 아래 복사 생성 master는 이동 하지 않음

# git rebase bugFix master
//to bugFix  from master 라고 생각하면 됨
```


## rebase -i

인터랙티브  
|옵션	|설명|
|-------|-------------------------------------------|
| pick		| 해당 커밋을 이용한다						|
| reword	| 해당 커밋을 이용하고, 메세지를 변경한다		|
| edit		| 해당 커밋을 이용하고, 커밋을 수정한다			|
| squash	| 해당 커밋을 이용하고, 이전 커밋과 합친다		|
| fixup		| squash와 같지만, 이 커밋 로그 메세지는 버린다	|
| exec		| 쉘을 이용한 명령을 수행한다					|
| drop		| 커밋을 삭제한다								|



```shell
# git rebase -i HEAD~4
//HEAD에서 4개위 커밋 아래 HEAD부터 위로 3개 대화형으로 리베이스

# git rebase -i bugFix
//현재 부터 bugFix가 포함하지 않은 커밋들을 대화형으로 리베이스
```
