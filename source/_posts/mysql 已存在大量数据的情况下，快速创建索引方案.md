---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZPDMDJN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T090055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQChzKR3YWhJ37EJzg9aLLXf8RECQw1khLdSKjeEoWpnEgIgeX8u1XzKfH4SE2YbLozn2aRvLluD69lbDkPY6qcpBcYqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABHndnKmizN%2B50uECrcAzuIVgT8%2BiCjw%2FUd1Hd%2FiQVL03Regs6aw6hLCeCzA4fgp00B0vuXMfCOguGucyCWNGPD%2FPdcanNmYfUvskkbMy%2FwV%2F3k6k9HzFEI3c11S49i1v5HhK9JRqblECXuI8D%2B%2F5ezPVp%2FFBFHEVgg9EURC%2FIyxoQayhruinRRZAPu9f1GtDt3%2Bu%2FvrBPHSFSn%2F31WPinng69y6FsGveC5fOVN5QvB8U88PUcTCO%2Fv27UpgOt8IRvuaJqOQVAPHdAYXjUh7cd6ehngjbsTxTIUdvBWQiH7l4swmdZYzPFhvSbt%2FErHF7TOqPUaWHDQ7UuRbwPIjVv8pPECm5I0xGbw5gZg1Kj2eYsFfi2T9SrbZo9414uppxTnzVLPgrf8q2dW13cL6nRMzdLX0LcQ6T%2B8n0l7r8oau4En3g8N%2F9AQAZlQjebTPX6976rIkfRWwkYfN0YG11OhwNRX8Sxvt%2BxdQFB5W4ckkFCn%2FyzHLPlYuG4bPyw0kn%2B%2FilRaeNlFpRR7IbV2sniOHQX4v8AdRAAha8Cb6dmQuxmxmG%2BIAdQkUs20ad3Are1mZ7HUKP72sRpVb8QD8m2kIpTlja0fMx5gZo4Tpo%2FcqMdmA2KIAwlV%2FNapx7r2KjcEWQkM29NhuRuiMI%2BTk8cGOqUBvXSrsP%2FQpZhYJE3GOVUy%2BCmcRg9fLxGONWMUGUvYPk%2FEi6T2Ds1X%2FSZLoVhIxiYvXGTTfukNZUM82RvkleluUELVjBDimSoBYnntvEx0dRHHJDyTPDrTYhsx6wVkpRuNuEOdJYUy4DjC0baoo5%2BfiHLntXRPVkn39SYiytnJx0TXcdzUC6NvFJ5dOrCdfv8FTY3Tipt8IBMx4QsOCp6gdxr%2Fw6Sv&X-Amz-Signature=a9166070705336bf75073e98928f56233060aceed7c1cbfc5e9a13bddebd91aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

