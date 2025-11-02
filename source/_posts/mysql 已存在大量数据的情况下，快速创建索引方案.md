---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7SEWTE2%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQDDKS6nHb2JwmdIskAprk4X02ntLta7zQGH85iT%2FeFUBAIgDFaQh2JAdcr7t6u%2FINVARwMPM75g9oycYZkDQDSx2A4q%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDPnPJRK1bIKc4LLxRSrcA%2FS65nFjSSvcHhydE0h3TAwmhLbDlQ3i8umltgrmcRsyQ5m4HfLJsl3wRECbrVWlw%2BCKpvW%2Ba4i3ov%2BZhiT9LTO4rmCaxAenO160RpAO0mQKm488H4nkDkWnmJ3Z3pS8Y4GaNRMfItxqBREn%2FxKmkozZGYpWtbiIkxRJ3iGkB%2F5WqpQnzk%2B0xtB77PJ8ipQF9Y7RqsLHa5Qh7stTcJLKZtRkjMqP6iWtqdxYmZ4Omm0CHDhKXOBXpsA3sELZWA8dOZ9syTHHpyKJxX2pBFaXDyTJTvFO3RR%2BsYzhjCW7%2Fq1fcG%2FIajH4q3I7g5an1CzWOoBa1EbjupMGyVfbz%2Bb5CPo2dLDvfXwJYQegzJpdTjg9sVdQ2l67BkBrXmhVgVEmw1TqlcO%2Ft4SOTw3XHOLys6izJVjtgigtq4Ptaa9Fvxqlp%2BK0%2FnE8s%2FeTNc9S5sDfos9wlzhwkJP6cDcWeFcgdSwZGesPz0o1%2BUZ7hg5kpNC3dYz%2F4Fu8kApnPsWF4LLTteHTxZlp35vEX%2FUZ%2B6Szh4w6tjnjBMzhVrZJLsKCMuFyqGPzF85tlWil1Jy1M7ELznDWQm3OG5D0ruXOxMWjkoez3awBOEdGrfBsqghrC%2FfPJxLjaVLk%2BgPyzh7LMKj6nMgGOqUBw%2F4jwMlHylYzJCBd8pIcEOO3n0OyzsR9sqlK%2BeYSaIyZ9ikp2PXuiWZHO7fXN%2FybE%2FvzpREUOgQ787kgVaoIUhU048rRJjDg963SDh6gxqylzUdFQ%2B6ZSHsduD6McLOj6qIiYetTwP%2FRG3fn%2ByPWy%2FMTKC6C4Cyu1RIn4LVuiUklIjc1zMYfq8MA0yimqYBoEdyjWEm%2Fy4YvzwvZgEYnDXRNcihm&X-Amz-Signature=5891d86e702f50f603dc175bdc12f19aa6c116908401e5961e0e8ef330e0e4c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

