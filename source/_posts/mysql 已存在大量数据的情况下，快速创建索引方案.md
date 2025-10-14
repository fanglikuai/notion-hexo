---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SC7FXUZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAW5NnIKBXo5Sqg33pEbBX60ZCIAPX%2BsWPHCScDJngIxAiEAt7hsX7Ou7cHz4QYuVlldIwahU4%2BMpzXImEWiNyelg9Uq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDBzmmSOL6KfN0Nu0pCrcA5KE1yf6NaR8bSH6%2FDdf4WeRnaispuGs%2BxJZbWNTA8iG%2FQTIcj%2FeYU0gwbZxcQfIPVKOwjoFwRQKQbhWpSFOMKLgh9OhNZ6Rj1MbVaMZ9%2FymoFmSTnL7nc92KSWQLiGiGkVYtWufKvvoWj%2FRm5ZvpoiFTT%2FN3X7Uxq6OS2oaZjqIA9jI2LGxrGNS8Kko6nZwbnuaJAanqoQVUhn1OyplBQEupyngnu5EawS%2B%2BWR4gWAo%2FxQi%2BcnHdzsa57MnCPlH6zRi6kDUgZPNd9NaHp9xJhlD3t%2BwujymQbeVoyeWb3hSLieTHrKT9NJpy74SJvgwtUtuE3tPzmXVM4OGUv7m%2F5s%2FtspUUqiW3KUla0%2B1XFGfqeHfB5CV6sAan%2BBdecCmDOwE1N2KlarNpSn8QJzKnITN3OQOUYtxh9ClQTIAIpW28DqWC1CSRptgo%2F9slv6f9pTuQPV4iLKqdbl36ovf7VVfkIMBfvcWpVt8ugUmMmxSwGXlc5osUj4PHCWZgBo7OOKLZuY5wCH1r0Ju4XFhH%2BTmb71PBQdiiU8f7b52SZkhNfCxcRv%2FesH%2FNOuE6fasZdyCtiTgvQtv45uKLJ6MnbnUKFfkXjmRqwLFQgUUYXTyGqFj3uOXlku7d5d0MP2kuMcGOqUBCXa5h4OQffKbA2EYfgcLLEBSXqe2BlVSMLRhCd7OAGsFXoq2XbRUjFbbx9BNXdVyDlHfk5spslP0Gm3Gxk1adVbQFlXj9a%2FhByX29uAYGwBQA6gMA5KqtiHryGo%2F6ohQ9BR1kjhIeMhzEhRzZAe%2FuaTm9vIKRsGV543Rn2evKg9hJPUqmp6VVFYdZqaxIus12q1JyzXXfuhXiAqT9RRIlzOiYc6Z&X-Amz-Signature=4ed98229c268cff214d23466ea54f80bc50e3e88bdf0eba4a7c4ff63b7cc9f66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

