---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662LZNTNL%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCec%2FzZXc9hbOPws0X%2BZqVQyId%2Bn43DBWUcImk0DnUsvAIgO7w9uIL0XE6XrATQzGwuma2FwoX4kI6I1qEIgq%2FuJScqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHmRWnvUG4l2by7H2CrcA6N2yTjmETbj2xwsnIfeEb3WyXh633fGlLbIwIA93mFnsVv%2BH%2B54R1PgksrHf6a91MnhYegaN6duLIOZ%2FqZCh3yKlwxDj%2B5eVuyxDdeAxcNndFpuQcGC5iSXCipyrCj2wDLeyFWu4AXKb95mhIonVh91QaXo0rGg7LEJC0SxB9o8abUZnP5PKMX0Q%2FNbxaV0rSdZMC%2BMnUJ0t%2BaFoViPXmsTOh0VViYtoYgM3YrlB5z6jhFu6ef%2BB29oiHyJ37PJPdZ0pEPrx8tTxrxZkFM2S%2BUKHEnDbKBDzBJxli8dWiVExEPfI5s7Te8Y8NPf2A3SYgyPCddVXSWVP69H%2FKUUsEjnMWqeFcH8W3Ms9bcbvc%2BxxWQ5LQGNMXTaQFYkMcWbsBysboOhjVb9q04Y%2BOB6ifN8m4amfMWLaR8OWp7pst9JENy%2FOBbB3TNIZ26lsnP8kqPfTM4FK%2FGAUw8uXUb1aZyNbpuGQxl90wtGjGW1s6XsHYpV4PNiWIclwGLZ0oXdO1Pf668S215RsxQOSGpqILrteOegR4eJsS%2FlmQqYCyvodCCoW4hDolIDMvmwZhMYlBj7E1mxeSgMakYidO1frxLrGQAOJdbbYVsv85JIS4V9ffY1xo8f%2FodA%2FgxQMNP6tcgGOqUBqVIzB91E4btINGY5PVCJCJqT4I66G7pFPeLQ0S14C1nwZx6lAmzn1vp%2BKNktAx3oLg%2BpsCMbbDIBSc%2FQndz68OZ8OHZ%2FMp6PYD1gNs07PDRtSiMkVaugdtKlIPQuAwxqq1tOvBwEJ2wAD1326HfURSpIXXXEaKSLL0ocUk3%2FtOCS5BwZ%2BR1Yz%2BS5Ch%2FirREor1FKPLPsEpUKlcQQ98TGGCa9o4PC&X-Amz-Signature=f6c64e1a84bb4500e42b871de7c93187e5cd245a7e70a6f2ca457ed2a62a5b03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

