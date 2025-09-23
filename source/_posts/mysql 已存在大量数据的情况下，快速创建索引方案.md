---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHG45VRB%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFjerAtq2f%2Bw95MzS4BHqIzXvVB8tVvGWTNZAzG8t7FHAiEAoT9PdTI2CEhPGb0GuyNwEyj1ZearXsgAgKBTrn5RtQsq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDG09s2RmhCd22egJFSrcAx11d8f2fRo9l4IT4%2B6umBpHrzxiTMWIrq65cXaFnjHgo2uaN7krKq5Se2VT7UGoUpdfYsm6DBkO%2FxaMVBVaVkmNmHRoL94dKiiPzlxX8ijiWkq1fupex2l4dM2yNB27mG7XVCCYsSs02nSoFErZNekH1O1Ei74y8bdd3bvIUvMWPEgZqZ0VmvOfjhc9nWhRMEI5NmqJG8kbeXa6QiowjjCpRxBvOTKC7MVSg16EnADmyezJ4tPfe%2FkO1rsqYIUnwWL88kGJVgjpyIkHumje0z%2Fv83aNZvTDGjELhS7efzPUUKsaJ5%2B4d2f3F%2F9BAwNEX9Yqkvn6ehiAaqCo9RkKZ8kijD1A%2BnZKtrgLRd26COdbgUiVIoZL5gOxzF2BpjO4jGnu1WZxxLURJNFpRxPMPZdifr6rMQTvQEndModOy%2BK%2FU0PasMuQxYjF1%2B56R3duvRNp0uOM8v3vZM9yYMVoJFlSrnjEmfG4Ewlfz4EXFyCacN0GRspA1F0QaarEG3lC14H12Z9XTdly7oaeE%2F%2FnKmiTjtGO6XYirZLOzp1zicPBzufwD13HymDQ3IDS%2FBnw%2BvGgKF%2FSwk3wfXOXDkdF%2BzNZRB5NgZaOLa6dTu7qb4fu0FuF4aVSY%2BA9ZBMqMP3aycYGOqUBpTQZZpkkuISABXhQcaYzBXfcBtoRjYk4j0I0wW4YFAYlrKn8y9Ay2%2FGIDuv5Wbe8VET1%2BKPRatVXyweev%2FpNl3HnkJsuTFK9t186SSWVH3GxvG%2ByKblyD2LP5tWszb5SpHDBItXmY%2Bh%2BolnKoaDUA4V245KCyPvCACOTShgz08mTxQ1wf2sXtLUt3Io%2FFOiLBghCEJkI61QkQ8ebblKskzdNH1lP&X-Amz-Signature=4d0d0462aea2a0134227740d50bd53cac3080897edc8e2eac91d77564e1113ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

