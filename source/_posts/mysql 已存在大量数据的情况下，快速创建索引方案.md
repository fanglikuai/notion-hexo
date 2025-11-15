---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677UMPNKZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhRCVosCAeIOe82WBGHltH%2BZCLDA5djryLN%2FgLtfX7NAIgQyLbTpnImb6gwfIuvrARBMqk2HqCJ6YQ9CSP2YpaI9IqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAmtKFy716R7mjBCjircAwpOs6AIRuHnzMkATLNUHkRPZl%2BIeBszhIVQWc72qdBsJtC4LQW5uVpIGXBk690VJug1Nw51jXOnYFhhIL3Rj60fDDRaEPYjFXSpYqM%2Bmijz2jGa0vvpE%2F3mzKoaeumbmW%2BxlGeseYty0VAQ%2B9ItZQNSzGmOsMPgTHSQn9SoXUwEna%2FHE4V5DpqQ0KE5n4UHwMUlE9t25qRq5A2gO2%2B8GyxKPicKW0iV%2F0RzuAmSTbAV1SQXwmip0k8Bg93jEDO9UMOBb7A2w7HphwrMPm8aO%2FuROAviCrQorE%2BQ6YGQhgCFj7evXH4En4mHRJYma%2FmIUJ9vs3IVrhibG6j0YqDoZB%2Bys8F8PR6LcdGBJza%2BoyophaIyRYOAC0GUyBp2Q8OFln%2BR6hM8YbU2%2FL0sfgGUfk3pn1%2FwnWDJzh2l8Va7Kis5M7rrBApHjMUgLl7Yw%2F8%2FN1eQGZFwkFLOVxtzqc2nFE2Ddb6wJAuXzEt7FwWLET6hctE4UsDmFk03WgnZGFkD58vzz5xVLcHltXyDIv%2BlsuwwbzzMvzglRjmWCbEgcJ3b3UllXQIlrbch%2BJ91WfEyLy5tm2skqAdIjLHJzKF71%2BCtZDUExvRkKSF7ICAkYEG%2BwWn2YZqpQyJgR54EMMii4sgGOqUBxn%2FRSdE9gCDTJbK0Fuogd%2BDvp6LMhznh79i9TwvlOVBZtvcXt1sYDsJTzqLB1a0gyTXXDCrvuFa8gqnKNl7hHarWbWP1uZj3Oe1AiqETS1ZAcpFgJmN7JkMtfbczyE%2B%2BqvWqon1HOoVw6aRMErqKiQ5dA6puOBIUPXdhLpJvNimeIxrciL6hnyV4pjLfINhDBSanZZa4RN%2BuB9v6aj7bCvYhuMEu&X-Amz-Signature=e0b138358d6d38d99ae5c928e5421dbe8c5c0b3f369744d24333471779aa973c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

