---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IERB3GO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpdIf7WDkPueUa5R9wkhtQbO6xZbUJYRtY1Xr9ORRrzAiEAl0lSuXKKiTrti4YOqOOLqDUOOVy9w1LU1lmAVRg3PiIq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDBs4T52LSD6ExtL0xyrcA7mKA%2FNk%2F%2Bt4PNHQnx6Qye0m1%2FykXdfmE1Ouuo8AiJSghA%2F4AtGhrc62YWtSdK%2Fx8wGqjd%2B3%2F%2BJLARTIiXZfoaa9XPaYXYZvpxx4KFlIXigMviwLYZ1foVnsqqQsgHrGiELw0vaMjUWpZ39t5nBTRru0Tx28%2FFwpdD8eXVtHjbFkNnFr2c41v9R8Boq9K7CUy7bkGE75Epvpufb3ndP3Y7q57tjlgBpDSecXmlHs0UXxmsPRfPETfCKav4uVSv3sLTM3Wy4Qb2s6PDaaGBuMUpHdiDNk%2Bz%2B%2B8kwfQ9US8%2FvzfOeMsFyBPzadCd3iMVEtlpXOvAn6VNwEuOmKQ%2B4h%2FzzT82DoWhw5ILlFfyfMnW4Wr4CamGwiusFT3xwfGRLZxbedlJPakZWsbhwQwbFyD96ztQJqAXtKIaP%2FlNjx7ta292VpcJGhrfbpB3044HIGX1nPRiiCAtI4BXUYSRWDmn4TFZPx1P8aigxNSg77%2BChVeQKGXZBhHrM%2FtwOasj069W1bMgYhWfKpbeL9ldSfaqILH1UCFeId9vYXgDSdtOnnHWXROsIgVv0eAii8zEFEzfqT5zXlppcbR5H54vHLaxVatPx9vkRUVk0%2F3GyXttSXJm4AalupJXPzZoA9MOLDosgGOqUBEBKB3TPAed%2B1wXvYVNqw8lyoxkVBHHFii%2BOgQJHXmbsVTRn0iounXroUV510MngqplWfqs5jPd8%2F766Q5HVSFZp0Tsyjp3OL1%2BC0aNfUslzyMgJg%2BdFZ2GC8LUibsblnHelfx0jzi8%2FHUNvJoc%2BivzZedAiEiLVQzxci0%2FJ%2BBbvV4irByoLUFdiq5seSp09IxMQMcqJrCW0WUBsD3GNrvi%2BEqgfH&X-Amz-Signature=f0a75c3929b662e983e328a8b445cc788ecc4ee8facf581957c23cbf8d261f12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

