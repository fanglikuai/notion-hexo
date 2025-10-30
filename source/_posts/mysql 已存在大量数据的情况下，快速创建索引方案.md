---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQKEIUZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T160205Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDjeM2dEJcAElGoK8aULNehqnz99qSribvbfj0rPZyXjAIgAoLjx44y8JvcGQl8v6E%2BJBJCd69tuUxHeM319F94IDoqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtZWu2oGl9NxikW8ircA33MvCMWvyPesI5Hq4G68psBcCJZiYnyY8QUlcSS4x3FGAYf7l25I4AS4WnyoLRXIkV3pjMhal5zTrPOQ3siaadDqDs%2FiWB0FnstXzpFmol5juQgl1ZvjEZeq2%2BoYD7vlBm6jwSAhPkh1%2B%2FW8fQhqw1pLGLyKaOYLD2gLjf9rBJgnqwNbRiqd1TrG%2FCO%2BR2%2Fz9XX5HGmEQMuw1kGSsKwqZWmF%2FlFJnyR%2Fc6Hw80GdMWoZvqiUADXRlgSzC7CpK92MS8rCaI2jjWska%2F8DTwN05gk%2FdJEmYhC6fzkDrX3SD%2B9RtC3pqkqLnhEYmLaoHPqCTBupDQtEreIrYDFfDitWKd6%2F41sr9nl0%2F3S2sf7Cyk8%2F7Y4Pcd8oM5IiG8DBvmtxZppF5Y1ndbrMbm4GudKJob3Oet%2FeHZiV5Tt32qiJJXIqSMJC1nXY5rQDcMQfOQMGk3VwaFj3JEh0rejU4ux5460PrT%2B2YzDQ2x2KUGuoiZjHSpKWuvVTMv1J0mI7jJg4H18YiNBtcHndhV0HgOlW8tucoW112oa4mF5y%2BYMFaU7n4AB5nLgdrGp2TeCIqXJJqb0P8n16kxrnLRHTAFeNDQuzg2qmY6lXt%2FGe8BTcnJeFR2HRVbMczqhoY%2F6MLv9jcgGOqUBxDvkQsmY9Mm4URlcW%2BFcn%2F2TttUiHGx35YGufuZ%2B2P%2BNJDN5LUimiUldejS6YFuAa0CmF3tekYYy0B3qeUSr0dNMiBA5%2BjPhT8vBvHY0j0Ybm7WMZ6LJymiZVgCtIETqqu3X0cQ07pLYj8SKoc2PuPaZeKQtAMkrkDEus7sP9IyXp7nqYIFZo8iIi4wH2MnhYkidkHlpeNEDYw10WSMaw4HfA7Uv&X-Amz-Signature=c3f83e05bb5bc07b09683a6b2e4915cbc1bc91475f7f9ec7a4f44532dadc5933&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

