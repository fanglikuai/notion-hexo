---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GZYHXJF%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGCrW08A9DgT5fUZjWuXfeO%2BXJZedPDcnxKv%2FHeeR42rAiA%2BNa3hq1KuP1G81soLaCJg1N1bauxEFGkZAoqu6FDBIir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMepR%2FUGGEnh%2Bk%2FaK7KtwDIAnEMEPnmvSw1HcXvt38o7igUxKWox4d8%2FE%2Fam7eVD0HfBZVr3LsJuDyFMssHHsxFydiVbAkhZB7xTjfsSq6wytFtmdela3Ks7DCz9SjN3Bup2jX%2FmGCEBgjexZvDl9%2B0vdbrtX5DzPfbQ62CmveE09rsOsgQDNDYvgsij8pkRjxS4D0RECu6O1tU0CSBoiR6m%2BrgvPfgTXEHk%2BFwsAXNfFvsSARNb8OMUr9y6AFCNVh5cDQckFIA%2FeBVYd%2BS77Lf4TNOsZGVhGHNskeY15xUw9b4rl1CRjXSIPIE6sXbfareoPjqSXVA%2Fq0WuEJag%2F%2B%2FxFSFtYvb2fjreUeg4RPtTIW9KnliYTDBVZ4WfN7%2FTUFqoKAXjfFGOJvGeSRyY2sg9TsqhozfzbKBveoT%2F4UwC2tA8jaHcLCoaeBJOTfgyb20RHsX3cOLmkPD5WcGB0U%2FrGDfbsIpOQgR2EcWkOLHQzXgy3O0Ar6VjrjaMI1udMh%2FBLAdNUXwwoOSjzKXWbZH4KhRbAwe2X6pn39RzEHzDjNMyNCG%2FNietDMlnawzsQzP%2F%2B8GjlUu66LAGaxTD%2F6iyBQiMpHI2e8K6jX64YK70%2BEB5BnTBIH29gVNwZgBQUqvqre7WpP8KB%2Fq8EwhN6CxwY6pgE8I8gCjokdpG3jTYbvEO9X8BLrNs6nH9SVKhrvs4NQ8Ne09i%2B10mRwPxbEgfTMPy0OtC7ajZegDJXKQPhLBxz7mpbf19d%2BxkydNBIllAEYvSLHQQEj%2BFOj%2FuwOld8RpIWl0%2FWFfi0Ia3r0ZMsW3bsjnpgbj%2BbCGKTC%2Fro1EzikHDZwsxKfDvzMKXFHdn4Pf0uNNTOqAXKzgwVfENkP7s4BzuD%2BjGp3&X-Amz-Signature=16c1711f062012ae2d45cedbfaa5b3d807b721e042827cddd48c0f4661db3333&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

