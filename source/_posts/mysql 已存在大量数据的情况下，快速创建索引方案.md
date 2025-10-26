---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HLOLDG6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3Kec986VIJVmtWKP5%2FkYEKiuk28MxMo6pYkyOIjACTAIgcSc8qD1g4OJga9ZGqWAkc5GtG8Agv%2F3Yt%2Fuy3SRqKmwqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBdDDazDTadm1ZES9SrcAwBan2GrlXOd8g7vjilTSi1SaHivf9rHnwNploAbB%2B%2FXHUqtaSPaLaXvBXkgU1AAgzT8FeNzpqUzLYhZWlIyaKA1sQRuK6Ty63d7kgQ3Y0pMfZ7QeOYlW6dm3tBWEHONU46U8BiNaSNZwGKovwuS6uvts2dJDmge1WF218VxWHbv8b9iO0gkZEUhiJaCi%2FSCMn59n1Piiyh1S6Wir9J0AV05Na39UJWnJqGwjKdDSm8l6Ab6T1UIexFv8XHTtvB92KqXNxB0HpKJA89%2BzDsjKpIMz25HEcdg7MSFUuNmS001bjINBRZprNHPP3sYV0DW4U6pfinhZWRLhz35tDEgT%2FYgoj7yfYizMWM6C3b6HotJ3VjbWi2MOLN%2F10xduBLal2st7Wk8FmUyh4RV8RCpkwYXw9oWBp2sY6mtxbEP6%2B1tBQYEb9c5l0IOMhP1IXfmfiYFgx1RtNwvKTeGzInUsCgoFNwGcg0DtgwMDhQMfnxa48v%2F85HZ%2BTK%2FyM8WTz5Wkp6AiLHySHrbyjzq%2FsCs%2Bobr%2FCX3lT81EW7cr5aQQ70gvGDaNM9Pw5Ns%2Bo7EBT9%2FpD%2FFsAXVpnE90zJ4qcaFZfddPC7x6EW4V7tjlGT7a%2B6Xp826sPKs1LmXneeiMIjw9ccGOqUBPb42B%2Bn4EPJvMIybp5v26UNo2uehS3w9ACOSstchM6nwlzkd%2BBRPfBwsFTE7l8pfLAbMZgOKvVOQpm0ojm2wlg0wze0ZK4Ve8WQCTiJ8vF1V%2FQHg7F00E0OiCKW8PVnIl%2FaxQLvYvEHumQc8ivGSurRFUc8Bn1Lw3eNHlK6KkhJI5PUECBUIyEbEHO8pC41lTu%2BluXX3bv6%2Btsv%2B1Bv4Hs3Lohw5&X-Amz-Signature=0e8d31b2897a04e9dfc3d3b235a1cefb4c66b904454c4c87057a20391d1648f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

