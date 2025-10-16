---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356C2PNT%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T140038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKIyldNZBg1aiFgQOjacE%2FBNi7k5sZoqp55SNHfVGI%2FgIhAInSvSklLNLketZEoXYVeyFsywUDYDLO0raBjQBV1nPuKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxnmLofKHmFsTh8Hr4q3APGjlDGdjDZPsChpa16A4iRE5Q6x4Lr%2BKzDJS%2BbETFP5yjceSqXfXhticG7VBlgN%2B%2B4h3q%2Fk1ERb6fip7aUei0zX7Fl4B6b%2Bzuhi6SeWBcRQzY65rGlfzL9z8IccYi%2F1NUmdR%2BVPGBo%2FBZJYzItJ1mX923y7kcTq7Kvg%2Bz6l%2BzHzk81IYqpgWBP%2B9Z5%2F0HVZvJayRex4yw35yyH2FgEN4RFoMRmUGLO3L8gYe50L0xaWlVxb1AWURio%2FrsJ00UmfSHEZ23L8auAMyAWpC0ioZEQsWXjvdvSSGY1m0awdc5yGTb1riVjG1t7EPgXcKuNaFgiUIVZEFSRirmtHuVdZM04qtJ0MhZ3Q2ci3yOioB9ROSW9ZButaUl9rxsH8Ai4g02xver9wDrWVAIE9YUHR%2FTzXyz55NzMyoce45BXgJ7zqCXxNgfXJ3xezrjqIJOhHkBPryGpF73cwM%2Be9X5bcz%2Fj7bepq3re0yCkJOEDje9aCHMeRiGYAZUMSCw1uN5kx69iPZqAlObfm8Y7rOwuUC4FJYatMrr93oLGUTWQZ5zQqahjTlGSvc20j0qTdc3%2BpyENAuRePifRs9p%2FOK7xQbSUEhCHw0HlyPKE1gj0W6C5IG2bYZbTuokNzDH8TDC56cPHBjqkAYW5OKWO8ltlFru7UomSuoVuTaiRdLs7clunjHBROIQd5eELOs67HTnqgRPAiJ0IN2UOIWGR8TgW6lkeMggLBSXxRSDPnMRY%2F1L5hesXQElb%2BLf%2FiIqMO2SRQ427P89ZXy%2FU48xdCSZLTUz1f2EMtRviOmggYJ635%2FYzPdLjY5hNUHRDSshbhVi3zMAivA4Xyy45KfMUeAORP40QocW%2FgUw8gXno&X-Amz-Signature=e2ea31d1638c85df17e2748e3ad467ca6752571c26cf2877a89d3e7716b7c7e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

