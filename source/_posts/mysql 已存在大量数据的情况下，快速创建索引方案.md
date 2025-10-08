---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636RHJIT4%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIBcLFJiMC4fiH%2FOKCgXc8Bm8Hycm2a%2FjS3AaOmMHr6JAAiEAngnrrbPJoVglCNPZnSwFi1vO0tgLIsEbhSnj59x2KzMqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHDRiiEc8GwWJ9RCJyrcA%2BnsK1PhLwNNfz3JXBUi1Evj7pe%2BgfBRCQexEyjOlm08MixPAuJXiGks3HqgUiGkEIvCTZhpfVkG9yaXoE7qmUXJZNTlnwh%2Ffy%2BnCi1plIJ38NDa4maLub5Oqw3YEgSmuc1oJFGqFjvH45%2FacGify2AjRdEhFWTzBwq%2Bsc3tfo3cu0K4NQk0UpgQfDC4BHBmt10OI2fqxo%2FRmtzwg0P5ngJbZkOe5bo%2FoJxotO70zuumDYD7U6iXES0bBLQsWMU6%2Fav%2BBTVxmulZegmk5QMwR6d%2BnCC1LK4jplr58RcPwk2v4AxeBUUL90zbJ4nAKajRZa3tsActH9zMpO%2BEa6oaOO%2F2%2FBMd%2FclW4D4SoZ9c%2BYp2QxdGHQbq5JPlFq3VIHjV9%2BqIIVQ69i3cvSnZWs%2B8q6u4JToElr5pny4leywGjDqJ%2BU3o6ZeLvRgReC4FD4gwYENbX5%2BtqzJ%2BO6KJ90zb99KC%2BNYy2%2FL5E8Dvh%2FnQb%2BtYqeRntLaLR4hngUawDTUHe28t%2FubrJFfBHAVSSBpm%2Fzn3yNKQHIQ10p84EgaxUlDI%2BdBzZpxjyEw%2F6Y%2FRmc%2F2oq5NDwCHKsNIIPEEyTWTXXzvcEYmfN%2BkKtCJqYb93G3m3PYxIjMmyKGtW3iAMIrqmMcGOqUBYdgmvHgXRL7LFUUX2bAPrAwL51C%2FxFbID1%2B16UYYiDD2xcxWeAzRwI6z07C%2FouhDgItXABVzrBkAvENgNVQNmnDWtuEh6f9PLLMazhCSXfHy%2FqCluim%2BXkDupZ1Fk4m8hG3eWBQfUrLDBgRmVHvzVXDIWwYEADJAl7%2B4lDXyFPckEgFnq7cHjPUEBYqwlxRKVyVM9Oja3%2FNDmRyR0g7F3x88pR5%2F&X-Amz-Signature=a1c6dae9dc435eaa67037d64a70ae07e149330bd0b493b70e2279d9ac8324e4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

