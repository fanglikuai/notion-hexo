---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGEQKXQ4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T040055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD1l8ukE4UhtSZHqPGy7qySa60IzYdrhMFTNIhBgGbiUgIgOo2GHR322DYZrKCWmTKyByfxpgjUB2yf4hGDM35%2Bffsq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDHSDFMLZS7ikYv0UpircA3X9cT0ibd7zvj190C11Y28Any8Vw55klJp3AuCeN0tRv9Aqh2FUa9vgJKPENfkzJ4hG81ydmWZMxZIRmeIy3d7X5F0GvmpG%2FCeRYPS67IESSqZ29PdLieAm8%2FWCY%2FqHm6BfdSkwtEA%2BKt9nY4IlkxchnCaqwDDu8RI2lnAfL0H5mrWt1KVn6Re918nruDzm4iga%2F8LOkkh3Rs2PvHrUt%2BmPdD8NbMrGh%2BH%2FqPMCSlTF4L%2BmBmGHTGqYAIt%2BFJ%2FyCUcceVRvVckD69DmtHDxAoqWQmnjdLGg6D03erc%2F1fH0gm8elB5uLsylMRuHFY%2F18%2FXNiVYRQUgEH6aoKg5HgT27c7NoiLvNh8pb6UoB6jVoFLPar2veews%2FxpBgjM1SMCI0vswckpNMpkCLcNbBN2LJMCKz3qtXv0Gi3dqQ9p%2BqF%2Bdp0IPzrdAbUvnhAXKBvARIJ%2BqyioOr6BTmGMDWvJJzrlF4QaHNbzRvScawIH0b3ZWqoZuZ4p8smKgJfs9uYka20FueIvawLj6nGGbbbQt3rKMZxyHthf93J70AYPMZaiGFOWVKCmG6W9yJkOQmZ9%2FLLGEmOAfB8bGN6974pasc7O25dCvI4lvn4EvHk0MXNC6RwRFFohvT7oFrMNuM8ccGOqUBd9ordSynTgiY1YVkWd1CjlqGTC%2Fj6nrxEEUAwl5p37d%2B1SPqHW3uvp0mwWKiu15sjcqPD%2B3B8W12iqlork3WIo5Dcf21l0ExVOGeUvYABO0BNr5IddeXM8sH8u3RX6%2Fvz1CBsh%2FO%2B7IXNwP2sUgE%2BcEGSiJbH6D7QXGeEvJ2F23almrhIQW7feucJhLV06Lz1G4PSYzib9WZA5aplVl5k7Om5OoJ&X-Amz-Signature=f1e249905635aa9fc0abff8400f0262cdabeae80c1d16a55fa3b1f23d9018027&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

