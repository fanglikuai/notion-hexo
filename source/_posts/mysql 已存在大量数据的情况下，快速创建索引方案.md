---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWKP4NSE%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIFApd%2FtWi4yUwlw5MOgffKAaK7LnPToSFRowfrDc%2B7ZYAiEAztASdUL8h1Rp%2BUFLqHqdDZRwnYqa8R7wkgPHM07t%2FI8qiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKz1YfXsBjYSn%2BC29SrcAy%2FuosT%2FWavINBmF54LUACJFXHBQlyRcAtXaCA9hOsXrDOwnZORRLAKMsz2RfgzYrKsxkfxXajQDDtK5SO2w80bq7LBlS7GT5aWV2T83twhvy6gAhIAT1YYFLBo2TWmrzQPBpplDPSPYZLvyzBj0JYJG%2BPIU9ZOY6GICefmNT6YhIAbgmJGDsE3SlXKmadQK9GEViB%2B8wjb5Cwq5y9mulcH7zLRBF77oUSaqet4lONf4BSMCyql3%2BYDslb7rJs%2FXplZbcwiAFDEyGIuxMi9jchJG1a%2B7mWJgZpKdIgmVyYlRrBUiUmTcNfBSWuQkjOKsIIFN4I65ArYTyv3fi5%2Bh4i4yPld%2FvBtIE5C3s5oL6wQds%2BIs33RF4QhiDUcoha26MAhf%2BqPt7voB6tz8R4I0m9DkY2tOIgYastHlgj%2BC6bMnB2Xk%2FmB%2Bdv7DblQldoFH0iLaw55NmsnADWPn9X5vwAK55xOMhCki5eHntIgKGegP4XbY2pL7DE0jIEc8lMgd6OQ0IPWrDi%2BelW5KrAKm%2F5TpvOqSGF4uUGHsuTmeUEAVpq%2BT6%2BAvf6lt7Xk40eEilzgg9I%2FtaW1oMZJ8ziwXYbCrVGpWL3msRNLcHOsLkjNpZ6%2FkeS8O8TI1cVrGMOrv5MYGOqUB7tvXISg8St%2FcPjstKJokRsJGDe5OTfYLjUOP1LyIjCh4KVSRTkmczbL%2FchsS32MhtkUN%2BNpDaC03Wn7z%2FYNTwKYKLeVwq1Vyn0BnPKQ3wKvmTKfI7ky7QZUxrcVdTmip93qgIHNpdmPVmbmJZ4R970mkLw5jMFFI0KAE92UziTkyEaD9d2Kx8MQ5dS4gePd2pJOnB3yjlNQk4GWl3rsh3i2Wy3QU&X-Amz-Signature=e7b9eee7676dad2ea6267369c25f731ead27b03104c0a2ca76ab2d8e84aa0650&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

