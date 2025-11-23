---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJYYOSDO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCICemnaAOBfldpppxwRa%2BWUhpYzXNUhjaRC1Cche5QsIKAiAJGkVFJRi%2BTotLV%2BZO2vQwPo0fJqHG8ES%2BxbzepZmBcyr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMH6uEGbKFo6%2F7uQA5KtwD59G%2F4rYNBccfi4o6Uvm8aK7nUufO%2BR9bFNs%2F2pBPM02EaSujmMoQUY3%2BA5hS0dyH3uKMxVshcxIqg9zaDKEb5HTZb%2BoNbK5UIhKpA%2FEWSXOQPYakK2a0SXkoGOgdPyPGQf4B%2FgDsKnWQU2CFj1APu5cYKNAOEkj24fCGW%2FBkdzJAjgLZ%2FD36lqYmBA0oMMxO2iunl7rSzHtSOWJwTmN6k6OvXKcRQ8YWegdKuCjFXALpQSN821XdtYgBgo%2BtXsgMbgtFoEOibFUfwT8n0EHoqG8TwPzXCheAjoC7k4l9E6DfwHHBlHuBJ8ntL8SvefIKfNltLfHtZB4FtvEc0Pb8z7m6z%2BApgvinjvhZ%2Fdof%2FZt3JkPZtEALainZYI8mbNoPrP6XR1m5JV4SfGTYg6GbX1cz769k93yxQ5JJlt30h%2FuiXRlDokXL2LEh6w%2FApbyzhvrARZrcieCV%2BRpj5D5fePF8LLMf%2FsK%2FCV62p%2BJZDsqiY66KHFGTNuSfB9D%2BvZovczACdgbQHmR3UP6XiYy8Ajy3UfkZN5xxyJj3QGDxOOGVYzarvX12UsPomc87oJUXRSsMSp3tXkxXkcGzo%2B43FBup3QQqZFgJ33ty3RVyTDCnFcpSau%2FZYW8fAyMw74GJyQY6pgHppQzmOxAden1PAwn7raoqChZZXCHHc1Un9%2FMl2G%2FDg04CNJU09x7445zpUo0%2FdcYEWAq3aqJ8aTb9e5XkpF1xHz81Gzr%2FeG4LCJ%2FxG1KnODmmQHLMa4OzMmYSFgzY93nHhEIMByE%2FupLk14IbQy89dz6kGGCAsRm3RV%2FfsgS8BnuQFbb8KoP0XsvwhBny1MbNyvju3%2BaIigOLvD%2Fhj6uxvkNnUwBu&X-Amz-Signature=9d8007a00aef4063662e5777057683ee553ade71a2156e016bacdfe74574cd66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

