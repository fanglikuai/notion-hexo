---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EY5VQ5E%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHbenExfNHSE29yjRNVWme2%2FEcZ7zBspWVMezTq1782fAiEA%2Fzw5GTy6Lo4kFOHVSELvajPYiMIk7tC5PII19Hsh%2BIUq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDB6UIY32gs41bULLZyrcA3oB8%2FMpK09W65LFB8K239lEGyUkV7kkXGz37xQl06PRtZ01NXOmgWXmmg1F3iXLwzdG1SZ82p2IXOIYS3aDqjCvdFhiORtaJZo%2B043whc7ovd8FxVu47PmTLb8NJUiX0k8%2Bfwqlpx9CVgdupsvLVQTmDIcwRChy%2Fe8QLacbxhRAxRdaS5TfMBiuwe1spCKSQOH1jaMpua2mvl8nknQaw9kKrffsgyl1xs%2Bj6LzABdo7QIWIGTKlzt9b%2FQKalYV0Nv1F8rg3biLH13sn9mEP4IotpEGXeXgg%2BSkhn7%2F8gO1q%2FzfDeNnKgXosCtmRylG46wtv%2Fjr9jqC346zkLfJKz25f24TRy6UUb44YLLtjnJHMQC843DSuYM%2F8kznPLbYZSM3leh%2B29%2FHLKFEtTBA7%2F%2FrHyeCt0rlbDU6UYmRSIjzXhWkYq3LDQGNqhlI%2B5gzWYXevzyxculxpv642MK8xDtonK3RZhg0JsMiFNZUdMiYWaOwY5kkEqyYm6cwmAzPZqKOO9VUne7%2BhAJUBH69o55mHTa4mubWFoqK0wkhrr%2BPKqNMrUwXI5iU4kob19IFJy0obmGQyyb7B6RZuyYPozPW%2FEN5fuQIHPZzBAWjmh7v9HviBBsuLRAipksSYMJOvzcYGOqUBN%2BU%2Bru5gp0Z9yEgoqJmXUqm6tsvWL4nCQybNbsFdncd4gkaWzF6l2XoOrw%2FVKRORS5v4We6fRKb4RcsLRxYHVdlGpBnNB57qp4xFhhhWXRsacwyxJBfSggshoKabERLPQiq1Sc2LcBzrSfTQzjbl12tzNLd1VrgYJ4TSt%2BDUSj7NAo3sIKBW3%2FLT1zFxhYqIO9Me8FXcK%2FFB042BGyZLsBqvHnLd&X-Amz-Signature=abc04a06dd96d7246e9f9b6a0e1b41cd4f8830e2755c6a0bddb24f1bb86b20d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

