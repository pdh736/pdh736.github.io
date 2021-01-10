---
title: "relog"
excerpt: "How to use git reflog"

categories:
  - git
tags:
  - git
  - log
  - reflog

toc: true
toc_sticky: true

date: 2020-11-16
last_modified_at: 2020-11-16
---

# git reflog
[저장된 HEAD 변경 이력을 보는 명령어](http://git-scm.com/docs/git-reflog), hard-reset 결과 돌리는데 사용 할 수 있음.

커밋 한뒤 `# git reset --hard HEAD~ ` 했다가 다시 돌아가고 싶을 경우 `git log`로는 원래 커밋 hash 조회 할 수 없지만 `git reflog`로 조회 가능

```
# git reflog 
// or
# git reflog | grep 브랜치이름
// 한뒤 

#git checkout -b [브랜치 이름] [커밋 해시]
```

## 커밋 되돌리기
```
git reflog

git reset --hard [커밋 해시]
```

## 브랜치 되돌리기
```
git reflog or git reflog | grep 브랜치이름

git checkout -b [브랜치 이름] [커밋 해시]
```
