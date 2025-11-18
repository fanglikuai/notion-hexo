---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU267VTC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGVhzZyjT2%2BuVQn3f0AhJtMotuYza6yRw37yR%2FjtdrQIhAJQ5ritpEESVVxwTWyN911OPREbcO2uw%2FhEsEJHd6TOGKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgylonThkbN8E9FSkiIq3ANOHGDncIIunOdw4vlzgU8UJTqP6lqSl2sNbKXpBMuq4009TYzor%2BH6zMrHnyUR%2B11epbU3gOt5OmKCGe4SRuF6JxlIkBbiR48HoQAnfTNql2RYcXAo80Uf0r8VqEtFnrxCxpOgsIRtjprZVRgzVQknA5aqsDTzQG8bdJsGwJq47Q6qOPFmx8T%2BRu4GUE4olWm2nKGvHLx3C35QLclgI1qbTSNKDUdh33kDQM6cVYO6gkXUTRoLtYzcIhLUebFTTB%2BKC7igxBvXlL%2BiOP65s0LWX4k65Je%2FGWj1gbiJF76RJcACTsd%2BKlY19Ayswaq1lsnP10kFrbllryyQfGYm95McB7lgXyzRZLV3ZYiUnnRg8qJtn7hZGfmYPh9ewxcpjwwrFGpIo3ndzCNmRG8F5CUJ2broGqQCe93xzamHNWzQQdWyhqbfewQVTxhBj40fAJPpYu%2FPpdlguUbtWexJpDlnI6bpLzJfVWxxTj0YUeKZIrG3Pllm2DSTIPBWPhXLFW8Ko6cS0sMgoA0JubaZKdtyOBPHBKC9yaACT1Zm4YW2YGQJD7ClNmfOwabZXP6RPx%2BUMAri9I62udkmfLiF43z3xQ2vGz9o0SJNCfES8Xz3ByAvgFnE%2Frl1G9P6UTCA3e%2FIBjqkAROSo5hN8OM3q8kM9v6iOcX4bk9KS3j2mfT%2FtNwA%2FXv5zDMdhM6r4OGJ9fAT34rQbxW7hfOSbI0HNJCw7y78AlvF0Z1AsdRItdjZTI2L86NyweXNLm83AF38MlZW4jNMACSW8BeIVQYk5rDvF%2F6ae9PxCVY6bKe1nbkbkW63o6vsrT6xcE%2FAEosiZzpAigthEPz6Rqj9PH%2F7bFKIbVDHYlaHJqVo&X-Amz-Signature=3f46996db875bb7f35906880a73ba4e235b16c32cffa9fed79566a5c5d56b8f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

