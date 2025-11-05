---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWJJUSI%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDT4aqlwVN8FRGH2rWYRJ%2Bwc6doyWQR6YqkMJZ16Gc6AAiB6HIA9Cyg6nxEd5XNijT%2BvjF8gtJHe7UUPqflWr0ne9CqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0cL9K%2BGltt2dx2BgKtwDOjtpbITm%2FGXpxWHirXiyGwty1Qf26Vl8GXfBhdsHAs%2FHl0VWhEsFoRETasllDZRSGFCDy82r3Tz8C0%2Bahm8fO9RjDCJlCtte%2FzKCIGzd4Ml7cZx85sY9p7Go7s1B1rsBI82juAWZsOFXhY52kfM9ntyEqtc9I39PscHK2MdCvHStgtEC1xw3HX8l2zxi6GuxL1gOsQWSemFUoDMSHgsR6K%2BwIsX9tgZ1Lyha2Ve0uAn8PVwKxuwpjscrD4E5BTsllQ0CJPpgrTUpjjlIVJ5ZhMDb6cjsxjV0ODo0wCIeOcbxH5YngYmV3OtcCX1AiiyIhf5huQrwHLPc3SxwDAMW88v7wi9M6t5BBbVfoLdYabdsyow8JB3O7icC05SCOdEBRZCSKdULodOcnkuBPnKP5%2BwgJQRUxPvoaa7%2FjBJRiXKED2XW%2B4koIFTfbtkttV%2FvmIEiMyYk9mAN93FsECQX01ujN1h7qxcxNdN5f0Avv7A%2BnnKriRC1oSP%2BWQF5qXw6vrXfJeiLUHzTotop2deH0j2pyY9uBEyDCKu4wqryQoHDRMW95uF%2FhHWe6beOiaiH3H%2FIRCusQAYxPEWBGXVuLpmgn5B7mmnbZNKsBUx3xhOe9jjRlUfcE9PsWBgwkfmqyAY6pgHeuTYfiRXtjfrBIP44cq%2BD%2B81%2BG80y8De3VEqbkw5W7VzMic31JwV1giF4MkNhXPDHyZLqJgDInxZaM93PwV83irUenQ%2Fa7MmcrTOpy2411LYXf1aFEbpfJI%2Fo6dJdYmDgWbOMlyZNyTCGKQNPZRrqe1pcOqha0Y0ectb7Lu2FGJ8%2BP0EeNJqfFpMzMqRMlMHXzNHPbBISFfpSJdSEh9RwftfdeVSO&X-Amz-Signature=baf1d91be74bbe38d335b946571cd96fc7417372d169659c7326500868ba049e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

