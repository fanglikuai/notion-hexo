---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X67OZNOK%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDYZYVfKXU8AbZJYoNyycoRp0%2BAokCAPL4zwQQhIi%2FjMQIhANx0ynt7CXx6r%2FdLAJzvTupbWp2weF1k%2FXK%2F7q9YPZOTKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzxeBTAGy7K7HYxsooq3AO6ilmqs%2Bif6xt%2B2hYlInbNSJ0fQD%2FcMLS9smQNS9EMdvkLftDYc47lZjmWrsg5xxbs7MjAcWszEE4rveAWOiG6y4vC%2BI0pFZEpV4iZlEoYsowyP1GwcPcW%2FO4hnEme70IfE4WFGSipcImZXhxJHr%2BgXZ68fWtD6Z5ZZleJ0Oj4KGjBhLENZG26ZciE6jBSPiR034VGR7N2Fy8M%2FnREg8NeEVSe1iLr9ilgk2T7b%2BK1f%2FsLz6Ku5Zb2SoI%2FZlBhVy%2FoxQJC%2B6D6LQkre%2Fy3BUofuwrjIw9peBvf4gPovAM6r8oU2cbkOdl5N8JOsUADOFK3k3KUgafdOwt5KmGSQawt6HB35mEzQGXuqwqLdo2vxsXlFrfhGoy40%2BbUED6u3VRGx7QwY8R0o2y6TP5i3j%2BbIln1URwFL9uADIol2rb4aeknWtf49lfREfuoE2tjYIHQ691sJQrh%2Ff6i6OSEQ2LizvkspOwM4EPKtU93HLpp3fMxT%2FL2%2Bunakzydig75ZQiApLSARRI5C4Cseudk8cVXIqmLlezusEwqycFPCZuIDenR%2F6M7wIyIstWjknd6ks6cuD0BVZq6u6uzLwSrf%2B0JUFWjLqNWQSu3y%2F14EdmxsfjHA8NnaT4RemnETTChk4XIBjqkAeyCh4AE%2FYTLVeTg%2BHwSalDY%2FabKuIRUquNfLMuDDUQyPzOmIFleXkS7ae4vugxiXGH4dIj6rIvf9pYMrC0bWJNtIFOKpfFvpPh9cSXzu11zPx5hJIfflcw%2FhuEWwPBmESBPdaFsAdblGDa5BtxVv2LOKYj1%2BMwUVwC0bwx67CUTREc9aaS9bXaiPu58JMPQPZlZ2mVyff%2Beh%2BWVqX8gBPsunb8h&X-Amz-Signature=55bebc244cc90b4de65220322ba30d0fb206aaa3720a8e8266222a5125723835&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

