---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMJ73YHT%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDA4Lb53K1hwd6Eo6Z5YbKBtH5n%2BZlVA3XEttHRsNCbnQIgX0GcWCI3%2BfvhYJ49mwzvc00RbHF317pIP6fkRafwRx4q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDCMI%2BaCR74O0mCUL3SrcA6M2F7o259cGLLc6884U%2FjPfifTOc8aV1sBd9tkNGZJS40IJWH3NGFxc5P8caWb8IeQitTE2oI1t1N1GvPhtvif%2BhYc06KNoFsCuJIm5SVg43Dc0xJLoe3HcD0t6a8RG29vHnkLG286KQfjizRfo8hiiAJEgWSDPEuhWVdL21nbUP49e%2BQXvhHDJhmPt0E3JY4hSPIzALmgJ87m228zFTE9IyTApWbYkb69xp18KRt8e3dWu9q%2B8So5DIjW9%2Fy47O6GDY2yFf%2BpybRw5hibAIyevTPO0lkfkskiHxf1V1X%2BxleipGPCG3y0hPJTgOq%2F7Xgr8LUEeb9FJArwRemv3abT98IhoSusllvjO51MKun6KGk21zSXkRKVo6xvvAevQx%2FCiKG%2BYyUtgho2oXnUnB3cWlCMckgI0AU1ggz7xRAfBhZigrCGhdLjy2WD5qUJS2Yr9Mxg%2FyrukTRtMisnsOesN4cGuJ1rVZsns8M9A0OKftXIfP3q3RxMCYgNIqTPiTu4yky1ryxJhNF0S46etmF36A6jAZu6PDDrmEnioasD4jbTS%2F%2BoNCzH0vi6pTLWL66rKy%2BhZn8vOEa2JjDQrMIo2DB%2Fdt05APpfb2JvLeQr7FbcEuX8s8es8Ifu2MLecg8cGOqUB7Vr%2FLE%2BDAkO42QqEmutV9duwF4hvBnAi0ZXUpbjf6LQmJg6XqE4Xitq5mRT4DsQO%2FMWCioW4jI8x%2F0DrDJe1D4dEXri%2BOg2PtIcNZp7b1%2F5DuJKZWpIp5zQb219tzJqFvqGLOFFwj7GqT0e44O0mxBpcIgThVaUFlPpnM9arH5aut2xz3soM0yyvEnaWJjMXATBNhRmm1ieHD%2Fa32eBYgBBHRgNs&X-Amz-Signature=bd43990327276568277d3530e91bf224058f9edf169858281f225cadbe459ee8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

