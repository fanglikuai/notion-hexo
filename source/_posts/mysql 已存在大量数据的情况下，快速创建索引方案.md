---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS5FHIFB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCrdgmePFiEmDuJgmAFp1fACvk2TylrH6T91DDZlTDEygIhAPH01yiFg5HyXTe93LBPHNX3%2FHkvd1dqN29M8QFwoX8rKv8DCHQQABoMNjM3NDIzMTgzODA1IgyOloRNByJI754rSeIq3ANc0d5szjtUHI0mfzFPxetL%2BR9dmldzwU%2FhvjbViNr7IJotZZHT6iYeVVy5HOMOA9u0PjuxdnJlmKrUtKmGinn5o7zsB2Neas%2F2G5DsQHkfjd0T6JdkA8FIIUOnxSQMHYs3o9GKJl1ebIHiCwZ2Rp%2BZB1w432WzA3RGnvRa6YXPZWo2AzYnzA%2FpyapGGbDkLI6mKQ7eQ8cPu%2BL2ZRlUWbL4jOZGnDrxeyzUnsZQowS3Kelz5Ixq2hbo%2BIlz%2BNTBurtKoY6uBl6uQDUtchiZS3W0eXuU%2FrrTzrXpaHU17SpRopuK2pTb%2BI2gR96261yBigWgufrMHwrMS6dWEJuv1bLMtlhDfrYllnciNkWINRoKY6IAodLWCfTV%2BDTtY7fcCIsWyuSXq7G6nM35fC4GlnnlazJUsB8HK6AdR4CT%2Bo1HtEJeji0%2BoDfuXb6ZrUDd0uyhM1GG5qw%2F9%2Bgq%2BApr27E6JOUJDrHeEMn44bSINLcRRGPMyDX3ZzW%2FToV%2BV%2FW4JOnKFTHCjrpLZU8LpEsfbPkiTYacxFavB8kl5WMeKH47%2BE5MEwyFv2KAFX5FdOKUlXX7OdGttQDOv5Wt6%2FTKAlbT7XvwQBGDZII9irDweM1tFUfKq6SRmPGCFG1BiTCpn4nHBjqkAUb3CuEcaS%2F2fqvYMH3zoBjzGmn4wpYdVU2C9Y2xycMJV2vO6dz3iDOuIyxq3aHHk7HwK2BROvtsqJXeEeWO7Vkv181l5lkuHZwh4JQoqMaqUehjsN7dVQXBWQChjJyqyen4l16GicKuOtcrz8uPf3UeMsRJJfZ9lQP4EPdyse6vunofzdFQBYGielobSaRJKwqVRR4ROjTYs%2BotDS1%2BnNMifETR&X-Amz-Signature=fac428312f05cb9ebc44fbcf9c6d0a23893476a571e22ae230c2a8a630689107&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

