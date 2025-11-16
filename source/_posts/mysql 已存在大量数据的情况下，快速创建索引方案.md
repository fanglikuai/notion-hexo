---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQ42PURU%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCyff120Hs7fsVJVX8gsUf9ohDCtIpYvolY7LkSNHayMgIhAMBHpMSn2zxp5MsCeXPq7fV4Wqxainp%2Bga0ckfb17v6ZKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwlco0gkYHDR%2BSBcRwq3ANfhg%2B9tF0iUBmKD7B5Ml94lX9vxwjBFWB%2BRDHxKH%2FNU9pWUsJdSUrJyKOnLGEoRUHmeGEEfk%2BttmVG0A7f6QZgB2gwhw4NrHuR0aQEBV%2B4oLPMUT3qU%2BDGlx4X6HRMsvu2RWISzRNjf3Bo08e8HZuKMUgdabM0IaYfG9ObwMX30LgceoMzi8VulTCToBkMwZlPQN807Bzl5AMgiTMbmMg4wbGlq%2Be1PXecrqharQuGn8T0DDQkEcemqUX022qLtilXWyHVnkIPuCJ4aDUUk9Tq%2FVmyUZKb%2FUKojomLjiJI6G8GNpFR5EO%2FtCSRGQCenJfg4TpCCgtKTp%2BnZPH0sqa1zS2HTR%2FYmPbVEqvBR5QqG8Gzue%2FoIFLCWx4Atukm%2FapU4dMhzDmva37jLcYIoYlrqLbAyy6JcnN24LFqZomGoP1IdNSb%2Fg1ELWVq3NFjmf0sGLcpeNcB50L7wiUCNwdRqAK%2F%2BLxQERPvNftF1yrQRjiZYjQv0V6ckNVs1oZMbtIdgO3HDO%2FTysuTsr%2Bu%2FZ2U3ou83bdYag8FbU3FNuqVbe4eqPGsobT3CidoodvRKp204JRk9hR3D4O68kmoD9AWblWfr7TJCS1Y1Mp7bRmcgb9XnYhnl1T45hOeLDDvzuTIBjqkAX32iVyxmhH5nL89p5Z6lUqI%2FbdkJ5o69Ya5VC2kNwlQbb11U8aX7WOWA5U9s5LhSq56wAzClUjRl2r9QlFEv9XG8N03qxwUXPm%2F4%2FEehiA6mdhLhDL2EKsMthfdESnNEGb8S%2BAK%2FTJcuTgA7%2FzmFML04R7SVCHo9qPaAwaWo4WI0BFQUXdUqeZNbvBw2W%2BsGexsjE4INzjcz7De6HmOMUtvOTRd&X-Amz-Signature=c6dbd440b33911c3f232d1340a7b0117f783f6a073f20bfdae4df37b6cf902f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

