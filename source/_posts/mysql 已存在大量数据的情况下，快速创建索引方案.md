---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGEQKXQ4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T040055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD1l8ukE4UhtSZHqPGy7qySa60IzYdrhMFTNIhBgGbiUgIgOo2GHR322DYZrKCWmTKyByfxpgjUB2yf4hGDM35%2Bffsq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDHSDFMLZS7ikYv0UpircA3X9cT0ibd7zvj190C11Y28Any8Vw55klJp3AuCeN0tRv9Aqh2FUa9vgJKPENfkzJ4hG81ydmWZMxZIRmeIy3d7X5F0GvmpG%2FCeRYPS67IESSqZ29PdLieAm8%2FWCY%2FqHm6BfdSkwtEA%2BKt9nY4IlkxchnCaqwDDu8RI2lnAfL0H5mrWt1KVn6Re918nruDzm4iga%2F8LOkkh3Rs2PvHrUt%2BmPdD8NbMrGh%2BH%2FqPMCSlTF4L%2BmBmGHTGqYAIt%2BFJ%2FyCUcceVRvVckD69DmtHDxAoqWQmnjdLGg6D03erc%2F1fH0gm8elB5uLsylMRuHFY%2F18%2FXNiVYRQUgEH6aoKg5HgT27c7NoiLvNh8pb6UoB6jVoFLPar2veews%2FxpBgjM1SMCI0vswckpNMpkCLcNbBN2LJMCKz3qtXv0Gi3dqQ9p%2BqF%2Bdp0IPzrdAbUvnhAXKBvARIJ%2BqyioOr6BTmGMDWvJJzrlF4QaHNbzRvScawIH0b3ZWqoZuZ4p8smKgJfs9uYka20FueIvawLj6nGGbbbQt3rKMZxyHthf93J70AYPMZaiGFOWVKCmG6W9yJkOQmZ9%2FLLGEmOAfB8bGN6974pasc7O25dCvI4lvn4EvHk0MXNC6RwRFFohvT7oFrMNuM8ccGOqUBd9ordSynTgiY1YVkWd1CjlqGTC%2Fj6nrxEEUAwl5p37d%2B1SPqHW3uvp0mwWKiu15sjcqPD%2B3B8W12iqlork3WIo5Dcf21l0ExVOGeUvYABO0BNr5IddeXM8sH8u3RX6%2Fvz1CBsh%2FO%2B7IXNwP2sUgE%2BcEGSiJbH6D7QXGeEvJ2F23almrhIQW7feucJhLV06Lz1G4PSYzib9WZA5aplVl5k7Om5OoJ&X-Amz-Signature=75ac0cd5aa49ab6580ddf2c8814caa35c7e9ba1a332a70aebe90a7b0d8577006&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

