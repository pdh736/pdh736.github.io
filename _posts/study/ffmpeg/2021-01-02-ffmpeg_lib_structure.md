---
title: "ffmpeg 라이브러리 구조"
excerpt: "ffmpeg library structure"

categories:
  - ffmpeg
tags:
  - ffmpeg
  - ffmpeg_structure

toc: true
toc_sticky: true

date: 2021-01-02
last_modified_at: 2020-01-02
---

# ffmpeg library structure

|라이브러리| 설명 |
|--|--|
|libavutil|난수 생성기, 수학 루틴 등의 유틸리티 기능 제공|
|libavcodec|디코더, 인코더|
|libavformat|컨테이너의 먹서, 디먹서 기능|
|libavdevice|캡처및 랜더링 기능 제공|
|libavfilter|미디어 필터|
|libswscale|이미지 스케일링, 색상 변환|
|libswresample|오디오 리샘플링|

출처 http://soen.kr/
