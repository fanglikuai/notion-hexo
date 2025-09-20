---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626O2BX5A%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCM5Oaiku3FZbfGRD4Tvz%2FUxEpppEE6gAIj47snS1OTKgIgSheOCYIaSVV%2B0vUnN6mat5zFH%2BqDEJxu4TGdaJ%2FKMLIqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBxVuGPxJ5OJTO55vircA242ASJd3crMINoRtYQwAhsXTUshnFIye7fH0a8vBM29vcXYkThvCFw%2BVqk8BB3giWSpYO%2Fk%2FkTkGEGaf2MJc2GzAgNPPpFkGP%2B%2FOLzKoMzSwFMCi4FwE27X2BWaXPd%2BbzABOb8%2FeGKNReKtYj2Lm6q9GTUopcDTgFWMSGTtsfyFaTkNzlIPiE3hCeuHlT9PyWqkzzR4DSOhyXJhOqJrPa2Dqrxes426Q6vaqe8C5er2wklWsVI64ciRgPvYVBssnALKV%2FOK%2B7aALPsgbB06Wx3%2Fa1UIua%2BCvn8W3XdpJ4BGhFkSqczK0tcaSnOXwYkwK8d%2BIqzdbqsI8iKUep0MiBKwKCKkC9SdHEGvA9qFZCOxH3loB3%2Bp%2B1nl1Cfx%2B2A%2FjO3NP2mr3A58jm0gBuzmf0o10UDpPnuARHDjqov%2BMQEZXNG7T5lGA4nDaVbI%2Fk2VvZ3ihC4f9WiN%2Bi0Q68W8%2BFsKA0hOHzXVLeK1VwVrz2AFqfkCXEx6C35423sdqpJW4DVX9a%2F%2FVdMzbeKVMVKpwA9vyrmtQve0M7nIm3zUdpYqbBVBkujpwx%2BHZaMQ58pYkM5JRRu9yA6G1ujcJveYUVQsoTm3qX%2F4WUiP%2BBw%2BPXjF%2FjiHeXM4uITIL9VVMPyLuMYGOqUBvfkznWD4RDo5GIrYw0JBAQTxXLI1XKPNZ7vCFueRo7dOND8yjTgL66ZyEUZr7q904Ndo3NmcJoBn7et75PHcjm7EY%2BFqJObR0nzj1gxMQULZdhd%2FgM6aPKG2BeTEXIYL9Szb7cwVMMICsu9bc6ZT7qShfh8GuSX5jczZdu68xJzywReUVBa7wd8XTTMDRAChxMaCVReSitAnD8gosGKEz6E%2FFjFA&X-Amz-Signature=87e2a2c4aeff171f59dd1c7a8a0c2eff024bcee0c7d24b6e58f7707999f112db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

