---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBYUASQX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T100058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaVX%2Fi7fkxofto%2F99%2F7CCaA22rZ%2BT3g51ZrUlcYPePOgIgMkfjn%2FgaTrEhwwAI4Ww2mAGEYwITQkuHj4ofwEPWDF8q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDLbWDJaL21ptxgSsvircA3HhALIq4SwrENfQCCpN69k3f4DVN%2F2ItvABQJwAGQ5zeUejIHN4DRLeDcJT75siaR3%2BsN%2FgIeT9HcEYAAOXrTwWB8heBJgxAGM0Q4mfj7w5tCAXYXPMzDYKXWTu%2F9o%2BFDFQoKMILhI1HpiuKG5X26i%2BzlpjaIq%2B7406CWgjI7W2iPbdSLlUlrbLAv%2FevRdt6C55nmB6uMWxldWQg3MB0EP%2BFwkEMecprmCgW5PLSGEMf1MZTQ6%2BsU37gr1PFyoqNTYvHZ4PUUwqGo09gqTO2V7FxOrPtgeDPDSNaa4RBdL9msZGlzQ%2FuaEEoy%2F78%2FjKsKJJBVgbW3z09WoAqvkEh81j7faoaLqGZs%2FuzyLS8Rn2kCpurutgtwLrJYZjYNSeqZ1mqkHuFA2B3I3yhgwXnOb3AvAtg6ZisLP54GVkUCUXBVmDfKml%2BjUYcbGoj%2BN0fQECzlyxdZNuFC%2FRtAFoqo%2FbCjtjjOFfn0ZHOxHPbSZAlT4%2BmHD2v5%2FB54klLpf5YfdRomoF6KlUrefxerDLJRBu4l9DdQssQL6GbyXRDgBmuSLS6uEFofqQeHq6ua%2Br91PIuUt9iYzamS5PiD17vpVF53b8b4hr%2B93tQU53%2BSRQ9yLMY8GjqvtbgarwMPzAkMkGOqUBlPevhsZtxuW8ifnzlvwwgkk%2BlivaJ80RrgwM7G4t%2FiTPaKAoVXBTOSJiMxAT3WwTfePYvE%2B2kLe80h1ngt30R7Qd0pqqSYClaYv0SQPxAEQDjSKs0AaaxrrQtAprfgq5fwKle1MAI%2FSqYBd7%2Bt41LBTLiDVVHbmc%2F%2FW3G8WbmS4IaCXHikUE8lQbJ9eYqmJXIKqAD3PN1JbIqyJqmT5CAr8V2jx8&X-Amz-Signature=500b5d663005bc355108e09576f7583974aceebb85310aa109a991432b62be03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

