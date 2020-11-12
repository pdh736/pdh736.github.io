---
title: "remote"
excerpt: "How to set git remote"

categories:
  - git
tags:
  - git

toc: true
toc_sticky: true

date: 2020-11-12
last_modified_at: 2020-11-12
---

# git remote

## 리모트 저장소 조회
```
git remote show
```
```
git remote -vv
```


## 리모트 저장소 추가

만약 기존에 있던 원격 저장소를 복제한 것이 아니라면, 원격 서버의 주소를 git에게 알려줘서 쉽게 새 리모트 저장소를 추가할 수 있다.
```
git remote add [단축이름] [리모트 저장소 URL]
```


## 리모트 저장소 이름 변경

`git remote rename` 명령으로 리모트 저장소의 이름을 변경할 수 있다. 예를 들어 'myJSDev'를 'myJavascriptDev'로 변경하려면 다음과 같이 한다.
```
git remote rename myJSDev myJavascriptDev
```
리모트 저장소의 브랜치 이름도 바뀐다.


## 리모트 저장소 삭제

리모트 저장소를 삭제해야 한다면 `git remote rm` 명령을 사용한다.
```
git remote rm myJSDev
```


## 리모트 브랜치 생성

**git push (리모트 저장소) (리모트 브랜치)**

새로 생성한 로컬 브랜치에서 파일을 만들고 commit을 한 다음 아래와 같이 push를 하면
```
git push origin utility
```
(리모트 저장소)에 (리모트 브랜치)를 생성하고 현재의 로컬 브랜치와 동기화를 시킨다.



## 리모트 브랜치 가져오기
**git checkout --track (리모트 저장소)/(리모트 브랜치)**

리모트 저장소에 있는 브랜치를 local로 가져오려면 다음과 같이 한다.

```
git checkout --track origin/utility
```


## 리모트 브랜치 삭제
```
git push <remote-name> --delete <branch-name>
```

## 리모트 브랜치 참조

### 리모트 브랜치 조회
```
$ git branch -r
  origin/HEAD -> origin/master
  origin/master
  origin/myApp2/develop
  origin/myApp2/feature-showTimList
  origin/myApp2/master
```

### 모든 브랜치 조회

로컬 저장소와 원격 저장소의 모든 브랜치를 조회할 때는 다음과 같이 한다.
```
$ git branch -a
  master
* myApp2/develop
  myApp2/feature-showTimList
  myApp2/master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/myApp2/develop
  remotes/origin/myApp2/feature-showTimList
  remotes/origin/myApp2/master
```

### 리모트 저상소 상태 조회

```
git remote show [remote name]
```
리모트 저장소 상태 조회 
```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:mylko72/myApp.git
  Push  URL: git@github.com:mylko72/myApp.git
  HEAD branch: master
  Remote branches:
    master                               tracked
    refs/remotes/origin/myApp/dev      stale (use 'git remote prune' to remove)
    refs/remotes/origin/myApp/topic    stale (use 'git remote prune' to remove)
    refs/remotes/origin/myApp/version2 stale (use 'git remote prune' to remove)
    myApp2/develop                     tracked
    myApp2/feature-showTimList         tracked
    myApp2/master                      tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local refs configured for 'git push':
    master                       pushes to master                       (up to date)
    myApp2/develop             pushes to todoApp2/develop             (fast-forwardable)
    myApp2/feature-showTimList pushes to todoApp2/feature-showTimList (up to date)
    myApp2/master              pushes to todoApp2/master              (up to date)
```
리모트 브랜치와 로컬 브랜치의 관계를 상세히 볼수 있다.  
다시 말해 어떤 로컬 저장소가 리모트 저장소와 track 상태에 있는지 확인할 수 있다.  
여기서 눈여겨 볼 부분은 리모트 브랜치에서 상태가 **stale**로 표시된 부분이다.  
리모트 저장소에 더이상 유효하지 않은 브랜치(삭제된 브랜치)를 로컬에서 계속 참조하고 있음을 알 수 있다.  


## 리모트 브랜치 상태 업데이트

```
git remote update [remote name]
```
리모트 저장소 상태 업데이트

### git remote prune

`git remote prune`은 리모트 브랜치의 더 이상 유효하지 않은 참조를 깨끗이 지우는 명령어 이다.
```
$ git remote prune origin
or
$ git remote update --prune
```

### git fetch -p
`git fetch -p` 명령어는 로컬 저장소를 최신 정보로 갱신(리모트 저장소와 동기화)하며 자동적으로 더이상 유효하지 않은 참조를 제거한다.
```
$ git fetch -p
From github.com:mylko72/myApp
 x [deleted]         (none)     -> origin/myApp/dev
 x [deleted]         (none)     -> origin/myApp/topic
 x [deleted]         (none)     -> origin/myApp/version2
```
git fetch --prune
