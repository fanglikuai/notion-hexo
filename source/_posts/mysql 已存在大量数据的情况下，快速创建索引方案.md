---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DWQSDDC%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqaNWzcQtoANzNzd3vizcJ%2B4URyXkybkYKQ1EFiJg1cwIgMMsIgk0i5M1%2BTu1fFI4NHmkr2c4MQaM4qa5YDCdEUT4q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDHhuLC%2FcjUko1oY8mSrcA2p88b7JO87dJQjdKs3i0dbXGuX8HMsxmmzXM47bJUKu0OJgn2QnkO3qXtyYuhefs32ZCOaoT4s8avi95PJEhhhbvncfND2SalLeimjFUuJPbDfAQCrMdI8KWVzUv7T2Eh444UC%2BRHvo%2BKg8rasLlX9n%2BEe36j%2BqPPlk%2FEiHCiqRBSVLMz5njy2MqvVjEGLvQrtwseSXyvdrNzGcJQP3xNv10fsXN%2FrD1nKZ1mjxspEVAwnmDu1%2BE74eiFvopH6mC0qeOcWy6lol8QZIxw6nO4LHsxe6oRRtfffIEpManhFIyNgQ6E%2BBOhC3Tn45vAE5QmymBRhiO%2FACeaJOcCy0ujwvukjK4Zq9ZV7blsO5M9uzKkzcXO5pKYKcJ38XEmuXZNZ6LRWuM%2BLvJ7teYidYeuI%2FP4OJOymJ4bRtx0g%2BBYUn4Gn2qTpuLMYp0TpSPXPl5g%2BywFMd%2FfhhCfijd1F9Gbi0N31od3%2FumAVrcQUOvJy4OFR2E%2FwVumP%2BY320l21cu3kQSBuYwNOlP0g0GmZHuqVr78A5P78GUUScaWcSk4j%2F9mt1uZqNoc%2BngN59mo%2FInX8feNPtAZVVOOQLbD2Ueh2MkG90ALIpP4f0dIg2F6gllMxkNI4MrMj22cVWMK2kv8YGOqUByA5J14bA6jHqjRCAUQa7oscgYHiMP%2FQmRa%2FzZe4PP7NAxumOJqSMaEM9GScFL0Fj2ffKWFyqMmU%2BWstNQVaC0YXfxIx5htfGN6zkiXvNrfNIXBcSsnx7ilpCy8wakSdONx8%2FwfQFup8jrXz3zcmNQ6E7Cx1uL7wOOl2jvFNjf%2FYbGS01hgaowJnbCIUPgJQ36Wda3%2BqRzAXKYpjjWtj%2BURKa1hup&X-Amz-Signature=32945b224affa342ea4f0225599bb0e0f88d55d88e4a06f2b1530658c0d5f50e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

