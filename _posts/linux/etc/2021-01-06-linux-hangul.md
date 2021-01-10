---
title: "리눅스 한글 전환 키 사용"
excerpt: "How to use hangul key"

categories:
  - linux_etc

tags:
  - linux
  - iscsi

toc: true
toc_sticky: true

date: 2020-01-05
last_modified_at: 2020-01-05
---


# 한글 & 한자 키 사용

한글 키 눌렀을대 Alt_R 눌리는 문제
Hangul 키와 Alt_R 키 코드는 다른데 각인만 바꿔서 생기는 문제
Hangul_Hanja 키와 Ctrl_R의 문제도 동일

## 방법 1 : xmodmap 사용
```
xmodmap:  up to 4 keys per modifier, (keycodes in parentheses):

shift       Shift_L (0x32),  Shift_R (0x3e)
lock        Caps_Lock (0x42)
control     Control_L (0x25),  Control_R (0x69)
mod1        Alt_L (0x40),  Alt_R (0x6c),  Meta_L (0xcd)
mod2        Num_Lock (0x4d)
mod3      
mod4        Super_L (0x85),  Super_R (0x86),  Super_L (0xce),  Hyper_L (0xcf)
mod5        ISO_Level3_Shift (0x5c),  Mode_switch (0xcb)
```
Alt_R, Ctrl_R 확인 할 수 있음(없어야 정상)
```
터미널에 아래 두줄 입력
xmodmap -e "remove mod1 = Alt_R"
xmodmap -e "keycode 0x6c = Hangul"

또는
~/.Xmodmap 파일 수정(~/.xinitrc 파일에서 modmap 경로 확인)
파일 없으면 생성
remove mod1 = 0x6c
remove control = 0x69

keycode Hangul = 0x6c
keycode Hangul_Hanja = 0x69
저장후 재부팅
```

## 방법2 : kr104사용
kr106 -> kr104 사용하도록 수정
```
vi /usr/share/X11/xkb/symbols/kr

default  alphanumeric_keys
xkb_symbols "kr106" {
    include "us"
    name[Group1]= "Korean";
    include "kr(hw_keys)"
};

alphanumeric_keys
xkb_symbols "kr104" {
    include "us"
    name[Group1]= "Korean (101/104 key compatible)";
    include "kr(ralt_hangul)"
    include "kr(rctrl_hanja)"
};
수정

alphanumeric_keys
xkb_symbols "kr106" {
    include "us"
    name[Group1]= "Korean";
    include "kr(hw_keys)"
};

default alphanumeric_keys
xkb_symbols "kr104" {
    include "us"
    name[Group1]= "Korean (101/104 key compatible)";
    include "kr(ralt_hangul)"
    include "kr(rctrl_hanja)"
};
default 위치만 104쪽으로 옮김
```

출처 https://jimnong.tistory.com/939
