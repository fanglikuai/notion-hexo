---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVPLTD6K%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQC2ASy%2F%2BWIxgsBF3xIiXKL2aduNQeWkB3pdXdESKka4ngIhAJM7BdhfJhqX5WsD90Z%2BGS2B5fJ%2Fe6me0WpLFr6aQa4CKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwa9Yck0piaLHHpXPIq3AMWISM4OgOvQ%2B%2BozwmPKjbUAMKB2tUHjD%2BVqHwU4XZzoz3NWNt0oBZjB8iH6yudFzUyLpZS%2BlLNC3mMUlVa9NMz5b7e2yCbDzFaAoFgPPERlHxJfjHUDH6%2BhusNGHJbUaDEPvQ%2FlQN1a1xK%2Fn%2Fid8COgP5CFcBNTSp2zGEXboCSYMi%2FxpE1ySyNu81LDykThm%2FBm2pCcwMTQ4sXn1jvoxJ8Accsqg2s0mcDK5lXYJWeqstUDZr8w%2BgQmDtBysQrjAf%2BcPV2G9MRieuamq30QVz2Vy0Crvs%2BX9AlgXqp5PRH1uL3sF5lkjSZpaftDJqDrYBI9kMwCxUCvdRVTl0D1hPTfClqzxYjH6Q7sV5Xfsvksz81Uq0VEQoG0H1anCHKGx6lriwPUOirXp4gMAgJlVB8DkyJ%2Fe4WGLB0JMNpAOzecKqnAvJrrdE8sQUWJTIP4zQsbQee2YwVOkO8MD%2FuzEVMhKzY9iGLy3Z3fdZkeB824jK7VApBXunzqcK4D35HWd2pfXX7I%2FHEG9OjM3M4FW4twT2LmHqCLjAsIsVxCDNX7cFGRJAbNu7U1wntsI5oMH6u317oIsyTXsymvvdcpiL5DdndwgDXd2H0OSwdUa81sV4%2Bm2%2BPCEUxGILTTzDwqeHGBjqkAYt4IbyHPiYmbRegYjiTgJqPrKWmJGa05K%2Bj5n0%2BYUJxEguFp0qQmC%2FT9B%2B8JQQaoRyQLUgpnm5Wpl%2BQxNI6aMihlCB7NXduA%2Fb8nqdMAd9YCcZj8Te1oafRRwCFUmaHQS5NeLrHSN8TpXBzxIkWGP93ZyXFD%2FwpgG9q8P%2BYDTPhaxNIFIBfhmnQ6JD8QJSXQNgAptuYya3JpjeFhwHg%2BkonaT2v&X-Amz-Signature=f8584097eaf4938ce464cec5cb53c6b6da7af306026bd09f652937e1dc28f672&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

