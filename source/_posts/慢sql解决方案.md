---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWIQ76JJ%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH8yrVEfbhSalEfDp8k8UCCwRUZtP1L64ftyKAYPGxkdAiAzWqa%2BWAg0ylCit8FNc7xykTqJ3ayipOmGKv07f2pwriqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP1XY%2Fum5S9fVQx6gKtwDj6PtNu3IjIkCnXV7mWs5ImbZkMAXVQt63kYBNkI2B1bRNW5gvT6RYvPL%2FAJRJms6WH0ESBfwdUga96Z2Bw08Tw97vjBcW8M7AbGP%2BnvDuJBZ6W2IKNFnK%2B5nzoBaaX1e9V%2B9qzF%2Br5oLqliEVgLw3EPMA4kJ2cdoMp1UUTEF%2FiZBwGn%2BJdT2mGX426xP6YLHOeAcrb0qI%2FB2BgcnE1zikAkrdLCkyPHM21mbRblWpQ4f6hZZIwLrUi9md8BruYfNbGcvie3EokM%2F06jPIkQxRT0Vl1NVMkmn36mRqrxRxnBSdfkJeMnuyEXn3NyHNeFv1RS961zAPmcVQKy0VzQWgF%2F6KEMiFrDYdEVo7s3fPY7aciIK0YOcBg%2BU%2Bg9cF55DEnH0DqwRgv6UZxgc2qsf01XTKY7wrrbz6U8I%2FMD49J%2Fib1WYMtyCNYSsR2aRNihm5Kk5cHllRPAwuEVBhP9eiW3mVOC4DJdwJ9nMTk%2FBagnC3WXg%2FHWwGxdpZIYCnasJoe%2FUitCBSmkkOFDF9Kn1ijrblVRlX%2FH1MXi81lMu6W8sFiDot%2BkJ9VkmG1CX93SXarRCTPBZs6EhuiqSg%2BHzLvvF8I5EHNSwLROYeBeeCs1oUlqU0Sf9ikKDoFgwspL9xwY6pgG%2BPQlJQ3erVcnaXlHYF7vazYczn%2BjLkqZ6Qwh3iHNPUbQeHVHS03eBXnGx6km92vdWsBJk4juc7S09zSHCvyUmGakKdknmRht1Q4WQQjYp2AY311W9vS88RaJA%2BriMOEjR5LuwqpLnP66g0Nilue35ZQ8cSjm%2F7W7oC2ZC%2BW%2BrTZmS5nPpexpniRSdoByJ9LJjKd62wHTAzHpANKLgU7kddyNHitOC&X-Amz-Signature=8bf67c2d3560cd9d8ff815e3ab9aa1649d116d153c94e99b8c75570977fd9e6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

