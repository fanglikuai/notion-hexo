---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKK4T7KD%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE7PBIQyzfQ4ok25InepgAgVEqFvWqmgHhMI2CmNUZieAiB1FLSzZpsmg9LvpubdeWLTVQ9EQatfbitlOJ%2FbiRh7OSqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6FQJnC22ooVOKUweKtwDz%2BHjMRuzOVJW%2BWNzbUr%2BZDoylTVIC0z91TJOUvxzOeo2VspEs4ZRljRauv1TBgTVnzbPEGhF6I2m58qm4GsKAwpXVnyyMuUkAG6Flh11AQoM196x3ln98fySFl0oCx%2BzQYkobEm5whmqMmdrkK62XrE0I9JrJ8Y5glf24UtpW9%2FeShvKNGpAsYjS5ZzqG61SZpP9M6sLM7UZ5oNGiPEaPcFnwexoVlqxgaZNXJfEG%2F3%2FNUjXnn1q6Z0WBts4JjdP9nvdsr8QxqlnhTqZ%2BpG6k5%2BC%2FkxDoE28De6MPY1X59DAeR3XrJaGGa8NAG%2BnnsbxlD96DQv1eebVsTT7ybdDZif6QLOxMTGGVKqIRLKY7qEeX4QZIiNvkD59TJVzeR5ImMFDcuyAiw2tUnFgdDrGuosdjOqebep%2BXMNFTSm43FGHkwZuiTHH9oHL9iNuDuuc72pd6KLHAsruqkb4GxcH0m%2Bby9f1B6l9Ct5yUdvvq0R8YrlMBdUrOTSqsxl4y5T9y6EqSeItIAzqrWfiSyHrwAddKqNsYHzGOS%2Bn9OGMYxJrlA0R7gU88rZe9uCxQfunGBTnE4wJq%2BZ%2FTe59KG8ZbLVNE%2BXtd5lBbQMuaRkZDrzKXAE51kSTp0V%2Fwbgw%2FIq9xgY6pgEmX4XhSLU36CXXy9LfMUPkrCfHUsnfO08zvhWCuIvzpDJnHZcmcCIPIbCN9TcululF9Kt%2B%2FnP4URgHgVh1WWMRqCP%2BWFQxhw6XRqF%2Ba80Ssc9PBvBceu%2F%2FJFGo99xogNvzpox5fwEttuFTh2rJcwCs5GO5B519I71nWZ%2B%2BHWGumU3K75B2A7uPQWTPbu2Qf8lXpGfOpxCoXsGANzRuYwgyuFN%2Ftn3Y&X-Amz-Signature=4915e988ba5d8997fdb4573e7e4bf0993ba15921c7665bed67157c2fed474d33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

