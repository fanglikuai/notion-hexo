---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q574Q32N%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQC4N%2FEacmIZEnx9Ss7tHwNFfc1fZaaXP874mYY4ww2CSAIgQxAK9JqdfPWCbp2iOc8o9FOreqiI6t6NkdmfJdcfO60qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLLIFl2R3BFwNgmv5SrcA%2BMtgOp4LxseLrhAJlJVy%2BumI2Ttp2QIm%2FGpUTmyZO2itfP9Tzuej9L9KQ0M%2B1Ga6W1zS4npOFsNH80kMZFEtH5DlytGpX%2BOrAGQ4vifgDQRQu86RryxG0gwflHtIYCtXZVuRPXSEqEUao%2FZRYhUqlCDiyEX9Djx81ygbZY9fsehwALxKajLbfEu2G%2F4awSb8E2%2FrdwttBCi%2FaD1QCO5i2uf%2B7Y1US748bSl6eWEGULMpwP9%2Foo7gNXE8%2BSsGyABW3wdivkMUVZZ9VZcaAxgEVMBqQ3BrVxre6VFTIOpnD0XI8ifHv3tDSPwbQcd8w0c%2FLNUUaBBo0r2uA4gjTUtxuPBkmmo0I7ganXg7lYGmEMQjySZiEhycgawD1C%2BArfKFYefjtxuAfP5hoOrmwoBnfktXjyy%2FZ4aB2dc2wA2sFqAaqXJLV2OmHPyX9tKbX0ZYx4U2OB8S2ZcYA7eFzOfDqObIwAhb2BU1hKw%2BNPyLbp4YI8eGvczK%2FyOREQHXOcWvW7CVhia0VCAucQnhrcntC96t1d3rFu8SEHa5u85KeS13Fcg9hMXKCFOlXSuDbEeiulP6BjvzLGTaileZwhRMH7eNPwyVrHOGNh8RQSfQjML2PD3s194aZxC9MU%2FMJyG7sYGOqUB7OxL3uIWuk%2BZlbIN958gmC55FuHax7kul%2BHtInuIG%2F3lysufENumBFM4qG826qycKedSAd%2Fhzb026UCguvnywkSo8cHgrMhBAJFLENNdCJ3Gu8CIsjWLbUiHmGA7gwdpYVU43d6jYYYsQnaAxNhHNSwOxtty2b1R5aMgOX9oG2Z%2BfSkUoNFowFAypWzPDdORmg%2FEbU%2Fbbpb%2BrLa5iINGemEkAhKh&X-Amz-Signature=df8bc4b4a66a3cb451db41aa18472e098c23aa390a19f392ce981d477047d050&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

