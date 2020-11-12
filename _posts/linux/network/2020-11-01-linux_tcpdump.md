---
title: "tcpdump"
excerpt: "How to use tcpdump"

categories:
  - linux_network
tags:
  - linux
  - network
  - tcpdump

toc: true
toc_sticky: true

date: 2020-11-01
last_modified_at: 2020-11-01
---

## tcpdump

tcpdump -i [캡쳐할네트워크] host [IP] -w [filename]

ex) tcpdump -i eth1 host 192.168.40.201 -w test.pcap
