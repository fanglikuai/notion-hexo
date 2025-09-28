---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBA2HMEM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDRoHUgzXLWo8KGarvtmYoRvK3YDDLVjQEiYci%2FVdF%2BbgIhANlzljKLJmqaswTsAHTs0oMp5cKRCHEKll7kvzWWxt9yKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxWEDMAgFmfdmC6uuMq3ANYqW9xmg2ZhHfuHSBfarEJf7Lb5HNEnH%2F4syjDwvIrcgu7mCt8Fuq1PFHSxsvF1wJpeQMAQMRgqPcvFYOx4la02CR5sZVSz%2FYm3h7G%2BCQ%2FjT2JI7QpmE8nEdL%2BsXOQDFKJNgbIQas7ZGUQ2zNn%2F3uMKPqM%2BOY%2F8GoJwig9Q55rNVMRTkXrgajTffPESEENuLYHNyF4M%2FOBnRaDjpAEoDMEz5%2FRIFtvim08B8mfKPbYfdkU84CjmytowlIkf71%2BL8WY0i2S1Z4pIuCNYMsGNfdzf4nDQiDXTTTc%2FJikLXCLwwYM45vlja3%2B0%2BdU5BPTcyZb7OyhNKk2juml5%2FkxceUAkAgP8PmQkmx5R3YdRHjzzaG4A0fZw%2BcXLyWOmolHgapeJRdjrWqpGyRizZRBTg4q2ukBgFhh9WCRwMX7i0I3y7Tt77%2FIAy%2BZQ7ebHXDkY45WonehiMSGCS9wf%2B0LHRFlSMg%2FOHgG8pOr1uQYfpr4umGZ5pka1W6Yt3%2BvPCOqfCwkerbHOfruMlgvTePz8us%2FzNYTzdsjQjf09yOt1eB%2BlYocFmAszIMeULixTQdIHtkTQnJ6udKK0%2BezrzS9jDRyqpPIUY76M1m3JwZPvZ%2BARofAD7Ejdccnm35hwzCq1%2BXGBjqkAY%2BwHjt%2BLvu2IDC9xiiuolsHYz9G9hZJRiVLVhWgoikuWcbA1Oj6hlVlddfESPLwJbj0m9doPlr1KKmvsdCEInjgj3QPN2yIZJG9gXLoNb0JDQ%2F%2FsIX%2BnlxMGDzavFSHdIloY%2FgOTtm7WBL0BFcobMLlQWWWsgiVfQfw0jjqz%2FtNaHSoWP01wJ3quLK8XLt3Gh18ZT5YEfivA7sYZXrghF3FM6Up&X-Amz-Signature=9455bb25b200393cc1e85feee81b196197fb8ab7797d6a4023bfcbb33329099b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

