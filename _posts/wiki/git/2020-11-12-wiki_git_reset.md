---
title: "reset"
excerpt: "How to set git reset"

categories:
  - git
tags:
  - wiki
  - git

toc: true
toc_sticky: true

date: 2020-11-12
last_modified_at: 2020-11-12
---

# git reset 
특정 지점의 과거 커밋으로 이동, 이동 된 이후의 커밋은 삭제됨  
과거 커밋으로 이동하면서 그 이후 커밋은 삭제되어 되돌릴수 없으므로 주의가 필요  
**Push 후에는 다른사람의 코드에 문제 일으킬 소지 있으므로 금지**(revert 사용할것)  


주로 사용하는 옵션은 3가지 : `--mixed`, `--hard`, `--soft`, 기본값은 `--mixed`  
 커밋ID는 앞자리 일부만 사용가능
```
$ git reset 커밋ID
```

## git reset --hard

- `--hard` 옵션을 적용하면 해당 커밋ID의 상태로 Working Directory와 Index영역 모두 초기화된다.
- ex:
  1. 프로젝트 디렉토리에 'text.txt'파일을 추가 커밋
  2. `git reset --hard 이전커밋아이디` 명령어를 실행한다.
  3. 'text.txt' 파일은 삭제되며 git status에서도 확인이 불가능하다.


## git reset --mixed

- `--mixed` 옵션을 적용하거나 옵션을 적용하지 않으면 해당 커밋ID의 상태로 Index영역은 초기화되고 Working Directory는 변경되지 않는다.
- ex:
  1. 프로젝트 디렉토리에 'text.txt'파일을 추가 커밋
  2. `git reset --mixed 이전커밋아이디` 명령어를 실행한다.
  3. 'text.txt'파일은 살아있으며, Index영역에는 추가되지 않은 상태다.


##  git reset --soft

- `--soft` 옵션을 적용하면 해당 커밋ID의 상태로 Index영역과 Working Directory 모두 변경되지 않는다.
- ex:
  1. 프로젝트 디렉토리에 'text.txt'파일을 추가 후 커밋
  2. 다시 'text2.txt' 파일을 `git add text2.txt`
  3. `git reset --soft 이전커밋아이디` 명령어를 실행한다.
  4. 'text.txt', 'text2.txt' 파일 모두 `git status`를 확인 해보면 add된 상태를 확인할수 있다.



