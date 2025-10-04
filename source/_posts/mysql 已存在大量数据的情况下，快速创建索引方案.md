---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OXK7GCW%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGEh3EgG38U4QTD1kXM8F7f%2Bn4F0p%2FV1SDWJ%2FrSW8b8tAiEA7Ebc5FXHMIKaGmodcJxP6KCeE%2BUs5RpbE06ihsnjFfoq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDDukwRLtWDODMtpWmircA9dzgzXsqDMgLuvLduMye2jAHVgaOdFLakoQtBy9LttypG5uZZrRVpgZgXrva9JfMKj1fU18ghTd7LI55T9oyzVu1cxGDSdpT5MqETwkYms0GwUhI0zpOxuCstkCbB4nBGhuFYHa1eLwHIcoS%2BKNupWSwgEZmntW9tCvf06n32UUKEbtjKXyybq86qLdIR4d%2F0zZRJhvjFtb17zlBG563yCM%2BfhV5TQRi9XsK4QYnr8VuvEuyx4PJq1TZesdDPNcqNn2SY%2BsnUXhjYfgJVtIPJoHFlBBEYd2yjYtqDalVhrJn2KEYjtksI%2FUYxSzQDphjbeaVQd%2Fg8MOnPhy3pLOQZ4NXgOmDoTuuF%2F1SVHdcCwW4APfUQuRJ97sYoaAbgc9C9K0js7nK%2B6yQlsbVWiqe6o3WTybz8mSsgFClXqNU4NfBy%2BB0yuQfjSJ%2BsngpteKPOM1nt%2BU%2B0tfLDtiZU0%2BioTXVq5cvM1z4krTHIFv7iu3pr4rSqRAhovKEM%2BPz0tmu9l7Eai%2B9EkM0t18KVH1sZAlDBSOYMqd2QmReIkNOxMan4PG61WeDOf8UhpikXDfzQb6RDy%2BkBT4R4vnWj%2B40P2GTQQ1JxH46LHgBwbX4zXUOJAKSQpBKx42yr34MLubg8cGOqUBb4kLrczwwl7dbPMlrOy3pHQVyk%2FtbYGOLAc8Cotgd3BEwmsiE4N4U64b6zYqjj%2Bv94vGbTk74SqNkrbmvXfaRzLrKMidBaAUNh%2BCQ44FB2WDzxrkgCUPT9J%2FQU3RPFvnne2lHZdo4nrQsn0I5s0wQnd80hQD9o2Z9yPkVh9UFu9vl9ESSfHiTvgbUvvYjWWWrd9OgmYOh9ZDNM7Q3fygTFxtqbEB&X-Amz-Signature=706f1b5559d7e9fab2288ba9bf1cd5f508a11775ee5e8d989109ab009aa01121&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

