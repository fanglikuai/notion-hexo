---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWKP4NSE%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIFApd%2FtWi4yUwlw5MOgffKAaK7LnPToSFRowfrDc%2B7ZYAiEAztASdUL8h1Rp%2BUFLqHqdDZRwnYqa8R7wkgPHM07t%2FI8qiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKz1YfXsBjYSn%2BC29SrcAy%2FuosT%2FWavINBmF54LUACJFXHBQlyRcAtXaCA9hOsXrDOwnZORRLAKMsz2RfgzYrKsxkfxXajQDDtK5SO2w80bq7LBlS7GT5aWV2T83twhvy6gAhIAT1YYFLBo2TWmrzQPBpplDPSPYZLvyzBj0JYJG%2BPIU9ZOY6GICefmNT6YhIAbgmJGDsE3SlXKmadQK9GEViB%2B8wjb5Cwq5y9mulcH7zLRBF77oUSaqet4lONf4BSMCyql3%2BYDslb7rJs%2FXplZbcwiAFDEyGIuxMi9jchJG1a%2B7mWJgZpKdIgmVyYlRrBUiUmTcNfBSWuQkjOKsIIFN4I65ArYTyv3fi5%2Bh4i4yPld%2FvBtIE5C3s5oL6wQds%2BIs33RF4QhiDUcoha26MAhf%2BqPt7voB6tz8R4I0m9DkY2tOIgYastHlgj%2BC6bMnB2Xk%2FmB%2Bdv7DblQldoFH0iLaw55NmsnADWPn9X5vwAK55xOMhCki5eHntIgKGegP4XbY2pL7DE0jIEc8lMgd6OQ0IPWrDi%2BelW5KrAKm%2F5TpvOqSGF4uUGHsuTmeUEAVpq%2BT6%2BAvf6lt7Xk40eEilzgg9I%2FtaW1oMZJ8ziwXYbCrVGpWL3msRNLcHOsLkjNpZ6%2FkeS8O8TI1cVrGMOrv5MYGOqUB7tvXISg8St%2FcPjstKJokRsJGDe5OTfYLjUOP1LyIjCh4KVSRTkmczbL%2FchsS32MhtkUN%2BNpDaC03Wn7z%2FYNTwKYKLeVwq1Vyn0BnPKQ3wKvmTKfI7ky7QZUxrcVdTmip93qgIHNpdmPVmbmJZ4R970mkLw5jMFFI0KAE92UziTkyEaD9d2Kx8MQ5dS4gePd2pJOnB3yjlNQk4GWl3rsh3i2Wy3QU&X-Amz-Signature=6e4113babd6cf97383a417825867e63a1abb84a88aeab235fef201e94abc961b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

