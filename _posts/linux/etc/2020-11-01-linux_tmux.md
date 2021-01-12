---
title: "tmux"
excerpt: "How to use tmux"

categories:
  - linux_etc

tags:
  - linux
  - tmux
  - xclip

toc: true
toc_sticky: true

date: 2020-11-03
last_modified_at: 2020-11-03
---

# tmux
Terminal Muliiplexer
-   tmux 세션을 시작한 후 다음 세션에서 여러 개의 창을 열 수 있음
-   하나의 터미널에서 여러 프로그램을 쉽게 전환하고 분리하여 다른 터미널에 다시 연결할 수 있음
-   tmux 세션은 영구적이여서, tmux에서 실행되는 프로그램은 연결이 끊어져도 계속 실행됨

## 구성
-   session : tmux 실행 단위. 여러개의 window로 구성.
-   window(tab) : 터미널 화면. 세션 내에서 탭처럼 사용할 수 있음.
-   pane(패널) : 하나의 window 내에서 화면 분할.
-   status bar : 화면 아래 표시되는 상태 막대.


## 설치
`yum install tmux`


## 명령어 정리
tmux는 prefix 키인  `ctrl+b`를 누른 후 다음 명령 키를 눌러야 동작할 수 있다. 
```text
ctrl + b, <key>
```

일부 직접 명령어를 입력해야 할 때는 명령어 모드로 진입해야 한다. 명령어 모드의 key는  `:`다.

```text
ctrl + b, :
```

단축키 목록(바인딩 되어 있는 키 목록)
```text
ctrl + b, ?
or
tmux list-keys
```

키 연결 및 해제 bind and unbind
```
(ctrl + b, :)
bind-key [-cnr] [-t key-table] key command [arguments]
unbind-key [-acn] [t key-table] key
```

### 세션 관련
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
(a or at 가능)(ex : tmux a -t)

# 세션 강제 종료
$ tmux kill-session -t <session-name>

# tmux 명령어 보기  
tmux list-commands 

# 열려 있는 세션 리스트
tmux ls

# 모든 세션(tabs,pane)종료
tmux kill-server 
```

### 윈도우 관련
```text
# 새 윈도우 생성
ctrl + b, c

# 윈도우 이름 수정
ctrl + b, ,

# 윈도우 강제 닫기
ctrl + b, &

# 윈도우 종료
ctrl + d

# tab(window) 리스트
tmux list-windows 

# 윈도우 이동
ctrl + b, 0-9 : window number
            n : next window
            p : prev window
            l : last window
            w : window selector
            f : find by name
```

### pane 관련
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

# pane 리스트
tmux list-panes

# 패널 사이즈 조절
(ctrl + b, :)
resize-pane -L 10
            -R 10
            -D 10
            -U 10
            
ctrl + b, alt + 방향키

패널 레이아웃 변경
ctrl + b, spacebar : 모두 세로 분할, 모두 가로 분할 등등 순서대로 바뀜
```

### copy mode
```text
# copy mode 진입
ctrl + b, [

# 블럭지정 시작, 방향키(or vim 방향키)로 복사할 블럭 지정 
spacebar

# 복사
enter

# 붙여넣기
ctrl+b ]

# 빠져나오기
(copy mode에서) q or ESC
```

## config
default 설정파일이 생성되지는 않지만
~/.tmux.conf 생성시 설정 적용됨
```
config 다시 읽기
tmux source-file ~/.tmux.conf

# 마우스 support - 마우스를 사용하려면 on으로 설정
# tmux 1.9 이전 
setw -g mode-mouse on  마우스 사용
set -g mouse-select-pane on  마우스로 패널 선택
set -g mouse-resize-pane on  마우스로 패널 리사이즈
set -g mouse-select-window on   마우스로 윈도우 선택
 
# tmux 1.9 이후  
set -g mouse on

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

# bind-key == bind (alias) 임
# C-x : ctrl + x , M-x : alt+x 로 설정 하면됨

# 패널 동시입력
bind-key y set-window-option synchronize-panes

# 기본 prefix(ctrl+b) 대신 ctrl+a 사용하기  
set -g prefix C-a  
bind C-a send-prefix  
unbind C-b

# copy 시 vi키 사용
setw -g mode-keys vi
# 2.4 이후
unbind -Tcopy-mode-vi Enter
bind-key -T copy-mode-vi 'v' send -X begin-selection
bind-key -T copy-mode-vi 'V' send -X select-line
bind-key -T copy-mode-vi 'r' send -X rectangle-toggle
## xclip 사용하여 클립보드로 전달
bind -Tcopy-mode-vi 'y' send -X copy-pipe-and-cancel "reattach-to-user-namespace pbcopy"

# 2.4 이전 (-t, -T 플래그 테이블)
bind -t vi-copy 'v' begin-selection
bind -t vi-copy 'y' copy-selection
## xclip 사용하여 클립보드로 전달
bind C-c run "tmux save-buffer - | xclip -i -sel clipboard"
bind C-v run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer
or
bind -t vi-copy y copy-pipe "xclip -i -sel clip" 
사용
-sel or -selection clipboard 없이하면 default x selection인 primary selection으로 복사됨(ctrl+ins, shift ins 공간) 
```


기타
```
# .bashrc, .zshrc 에 설정시
# TMUX 변수 값을 확인해 중복 실행을 막는다.
if [ -z "$TMUX" ]; then exec tmux; fi
```
