---
title: "tmux"
excerpt: "How to use tmux"

categories:
  - linux_etc

tags:
  - linux
  - tmux

toc: true
toc_sticky: true

date: 2020-11-03
last_modified_at: 2020-11-03
---

## tmux
Terminal Muliiplexer
-   tmux 세션을 시작한 후 다음 세션에서 여러 개의 창을 열 수 있음
-   하나의 터미널에서 여러 프로그램을 쉽게 전환하고 분리하여 다른 터미널에 다시 연결할 수 있음
-   tmux 세션은 영구적이여서, tmux에서 실행되는 프로그램은 연결이 끊어져도 계속 실행됨

### 구성
-   session : tmux 실행 단위. 여러개의 window로 구성.
-   window : 터미널 화면. 세션 내에서 탭처럼 사용할 수 있음.
-   pane(패널) : 하나의 window 내에서 화면 분할.
-   status bar : 화면 아래 표시되는 상태 막대.


### 설치
`yum install tmux`


### 명령어 정리
tmux는 prefix 키인  `ctrl+b`를 누른 후 다음 명령 키를 눌러야 동작할 수 있다. 
```text
ctrl + b, <key>
```

일부 직접 명령어를 입력해야 할 때는 명령어 모드로 진입해야 한다. 명령어 모드의 key는  `:`다.

```text
ctrl + b, :
```

단축키 목록
```text
ctrl + b, ?
```

키 연결 및 해제 bind and unbind
```
(ctrl + b, :)
bind-key [-cnr] [-t key-table] key command [arguments]
unbind-key [-acn] [t key-table] key
```

#### 세션 관련
```text
# 새 세션 생성
$ tmux new -s <session-name>

# 세션 생성시 윈도우랑 같이 생성
$ tmux new -s <session-name> -n <window-name>

# 세션 이름 수정
ctrl + b, $

# 세션 종료
$ (tmux에서) exit

# 세션 분리하기 (detached)
ctrl + b, d  
세션 분리시 실행중인 프로세스들 종료 되지 않고 계속 돌아감

# 세션 목록 보기 (list-session)
$ tmux ls

# 세션 다시 시작
$ tmux attach -t <session-number or session-name>
(a or at 가능)

# 세션 강제 종료
$ tmux kill-session -t <session-name>
```

#### 윈도우 관련
```text
# 새 윈도우 생성
ctrl + b, c

# 윈도우 이름 수정
ctrl + b, ,

# 윈도우 강제 닫기
ctrl + b, &

# 윈도우 종료
ctrl + d

# 윈도우 이동
ctrl + b, 0-9 : window number
            n : next window
            p : prev window
            l : last window
            w : window selector
            f : find by name
```

#### pane 관련
```text
# 패널 나누기
ctrl + b, % : 세로 분할
          " : 가로 분할

# 패널 이동
ctrl + b, q : q누르면 화면에 숫자나옴, 해당 숫자키 누르면 해당 패널로 이동
ctrl + b, o : 순서대로 이동
ctrl + b, 방향키 : 방향키대로 이동

# 패널 위치 이동
ctrl + b, { : 패널을 욎쪽으로 이동
ctrl + b, } : 패널을 오른쪽으로 이동

# 패널 삭제
ctrl + b, x
ctrl + d

# 패널 사이즈 조절
(ctrl + b, :)
resize-pane -L 10
            -R 10
            -D 10
            -U 10

패널 레이아웃 변경
ctrl + b, spacebar : 모두 세로 분할, 모두 가로 분할 등등 순서대로 바뀜
```

#### copy mode
```text
# copy mode 진입
ctrl + b, [

# 빠져나오기
(copy mode에서) q or ESC
```

### config
default 설정파일이 생성되지는 않지만
~/.tmux.conf 생성시 설정 적용됨
```
# 마우스 support - 마우스를 사용하려면 on으로 설정
* setw -g mode-mouse off  마우스 사용
* set -g mouse-select-pane off  마우스로 패널 선택
* set -g mouse-resize-pane off  마우스로 패널 리사이즈
* set -g mouse-select-window off   마우스로 윈도우 선택

# 터미널 모드를 256컬러
set -g default-terminal "screen-256color"

# 활동 알림 활성화
setw -g monitor-activity on
set -g visual-activity on

# 위도우 리스트를 중앙정렬
set -g status-justify centre

# 패널을 최대화했다가 다시 되돌리기
unbind Up bind Up new-window -d -n tmp \; swap-pane -s tmp.1 \; select-window -t tmp
unbind Down
bind Down last-window \; swap-pane -s tmp.1 \; kill-window -t tmp

# 패널 동시입력
bind-key y set-window-option synchronize-panes

```
