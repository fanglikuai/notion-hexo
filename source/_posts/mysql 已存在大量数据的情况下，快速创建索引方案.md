---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662HCSQQ2A%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJIMEYCIQDHrag5Ka4Hm2R%2Fuv4lLgXTHFFYCibuK3b9jT%2BgXQx19gIhALAHJJVz6mZvWyo%2Fr6JT8JwfqDctql7kjV2EHJQ9oeLoKogECK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww17CSBOb%2FkmAZxn0q3ANPhnfWCNPbXQQrHMTyVO54mzMtZxZu%2FXqhiySq9ZhiNR%2FGCdY5qNLAQ725SbUa8JO7INYPcdja1l7wRquM185yGuOZQDWNoPd3pTUF4a5%2B%2Bi3M8SzzVPbeXX%2BNueoW%2Bwb3twNbkdTa6wCjCmMvXmLPIz2yMhV6qm1Jd%2B8tiJPy1RijqSpaGJDBipHvaLPdeLmsVwOxM0LmDpVZCr1NY09sXHk3GckzEiwavXZNTL%2BilpGawRsD41HQw58pKOxu0H8HE%2BRoFkUTfP727xjcPeQN2EGyENkVCcLn3w2ETUeIJPrnma3%2B%2BYdou6glPrkLECzjQAzNizatOihzTWYScZAbgYr3IxhhuQ0nXglZu%2F6wCS55VBx2K9GUvNNFtUz2rcZwvRNwTSFVCySN7qQ97vNUOXJJOOadaa8Ok1xga%2BLksrURXie5dFH2CJYKoYV40iD7aeih316JIf%2FtCBUISAmyl7TFgcOVTw8oQ4d64PUuEpl71NZP7EogfQAQRLCEpzsaFySP8iUm0qRmOAaEOfPI6T6jCaYJsZE0G4dOsT%2BW44db7KMZFYObSDm8j9Jzi5%2B0jZ%2BfI4keRw%2F7YNqpN864Uw%2BCt0ZGJXdzr5wTYM28ye5WtvVMA5nl7Tf8aTCg%2BMrHBjqkAZ16JDKTmDUOstxI47cRoOm16AsztnoR8%2Fjn3EF09l%2F1PyowAYLhbsb2Zuz%2BSYjDNpo0QoCYsFDW4MiNscp9Sy2Lxpi%2F0WhDXIhf5AMhtUpBSbo0%2B9VTAv18F33mdaqMh%2B1cGu7Ljq5YDJEraaZQmhrhYiaRmw0%2FekDwJzkK0YYwuABeaXtczRbNAwTubGRvcgp7UH7byNF4E%2F50ezFQgKQAHuYL&X-Amz-Signature=92d722dfce22c6ef3b3d97709c91f3282bd964f23c733c409bbc12f857881eea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

