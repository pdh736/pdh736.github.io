---


---

<hr>
<p>title: “tmux”<br>
excerpt: “How to use tmux”</p>
<p>categories:</p>
<ul>
<li>linux<br>
tags:</li>
<li>wiki</li>
<li>linux</li>
</ul>
<p>toc: true<br>
toc_sticky: true</p>
<h2 id="date-2020-11-03last_modified_at-2020-11-03">date: 2020-11-03<br>
last_modified_at: 2020-11-03</h2>
<h2 id="tmux">tmux</h2>
<p>Terminal Muliiplexer</p>
<ul>
<li>tmux 세션을 시작한 후 다음 세션에서 여러 개의 창을 열 수 있음</li>
<li>하나의 터미널에서 여러 프로그램을 쉽게 전환하고 분리하여 다른 터미널에 다시 연결할 수 있음</li>
<li>tmux 세션은 영구적이여서, tmux에서 실행되는 프로그램은 연결이 끊어져도 계속 실행됨</li>
</ul>
<h3 id="구성">구성</h3>
<ul>
<li>session : tmux 실행 단위. 여러개의 window로 구성.</li>
<li>window : 터미널 화면. 세션 내에서 탭처럼 사용할 수 있음.</li>
<li>pane(패널) : 하나의 window 내에서 화면 분할.</li>
<li>status bar : 화면 아래 표시되는 상태 막대.</li>
</ul>
<h3 id="설치">설치</h3>
<p><code>yum install tmux</code></p>
<h3 id="명령어-정리">명령어 정리</h3>
<p>tmux는 prefix 키인  <code>ctrl+b</code>를 누른 후 다음 명령 키를 눌러야 동작할 수 있다. 다음 내용에서  <code>ctrl + b, 어쩌고</code>  내용이 있다면 tmux 내에서 쓸 수 있는 단축키다.</p>
<pre class=" language-text"><code class="prism  language-text">ctrl + b, &lt;key&gt;
</code></pre>
<p>일부 직접 명령어를 입력해야 할 때는 명령어 모드로 진입해야 한다. 명령어 모드의 key는  <code>:</code>다.</p>
<pre class=" language-text"><code class="prism  language-text">ctrl + b, :
</code></pre>
<p>단축키 목록</p>
<pre class=" language-text"><code class="prism  language-text">ctrl + b, ?
</code></pre>
<p>키 연결 및 해제 bind and unbind</p>
<pre><code>(ctrl + b, :)
bind-key [-cnr] [-t key-table] key command [arguments]
unbind-key [-acn] [t key-table] key
</code></pre>
<h4 id="세션-관련">세션 관련</h4>
<pre class=" language-text"><code class="prism  language-text"># 새 세션 생성
$ tmux new -s &lt;session-name&gt;

# 세션 생성시 윈도우랑 같이 생성
$ tmux new -s &lt;session-name&gt; -n &lt;window-name&gt;

# 세션 이름 수정
ctrl + b, $

# 세션 종료
$ (tmux에서) exit

# 세션 중단하기 (detached)
ctrl + b, d

# 세션 목록 보기 (list-session)
$ tmux ls

# 세션 다시 시작
$ tmux attach -t &lt;session-number or session-name&gt;
(a or at 가능)

# 세션 강제 종료
$ tmux kill-session -t &lt;session-name&gt;
</code></pre>
<h4 id="윈도우-관련">윈도우 관련</h4>
<pre class=" language-text"><code class="prism  language-text"># 새 윈도우 생성
ctrl + b, c

# 윈도우 이름 수정
ctrl + b, ,

# 윈도우 강제 닫기
ctrl + b, &amp;

# 윈도우 종료
ctrl + d

# 윈도우 이동
ctrl + b, 0-9 : window number
            n : next window
            p : prev window
            l : last window
            w : window selector
            f : find by name
</code></pre>
<h4 id="pane-관련">pane 관련</h4>
<pre class=" language-text"><code class="prism  language-text"># 패널 나누기
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
</code></pre>
<h4 id="copy-mode">copy mode</h4>
<pre class=" language-text"><code class="prism  language-text"># copy mode 진입
ctrl + b, [

# 빠져나오기
(copy mode에서) q or ESC
</code></pre>
<h3 id="config">config</h3>
<p>default 설정파일이 생성되지는 않지만<br>
~/.tmux.conf 생성시 설정 적용됨</p>
<pre><code># 마우스 support - 마우스를 사용하려면 on으로 설정
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
</code></pre>

