---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIXNGJOJ%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIGl869kOcyKauzSHBAML7ox%2BcZnU6Ovu39gedKUUzftAAiEAhw3SNnuyDr0YCfpp8pP9fiJGsfqDTAZZn4QZjhBzC%2B0qiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBRR8Bxz%2Bjbak4L65SrcAy4A7KjWYn685bc6DgEjAXT9uesFHwLW8izqdFbE1FeZ64K51bjScukRzXx%2F59%2BgW9x9NzPB9y7ZSWC87449q%2FFD64Q2dxu9WjK8LVL0NxLsnWmc8cVa4uX1oKwHcYwDO1NyMevyCnzkeF0OFqOpL0%2F385bT7N6x%2FKdWpTiGo77FzToiYIbgpMtO151mthb03FmsbBOA5SeGrjUcxMIvlYaB2DQ0RgdkibWqs4%2FBeMZkCGCadLyAPhrlFUV5SDWwXykwuGw7eRlqBdibPbzSxrCchzoGjpqzMafbVulmuJwVO7xAX7xdLjCTqYZ3GCmW3GRcnxD6K1p7ae6xbFFiGfDG7qZmbMbSnGuJBZVIFk5imtfeQrdGqddnBQBtA7eR%2B7Po4u9%2B832OJXzd81IYORM39Uvv1qLQutDnmkCSBmRRPO2iuFwMNpvLsK8needuL3MFHHIJETE7tEoXKE2DR7LgV2pRNV8cpEO4Ios8QJK0%2F5N8bun3077vb6FSjKflF%2FCBXtPVYZZtM1W9AESrWLYihDWaFcTNBVxJbru3b%2FCoIE51t8wd8VQIeYRn9SJ7Z62q%2FBFN7pbh5N7h%2F9f79xWFHOcVa69mJnxJw2GjmMs40PYzp5oclvAWqftwMIOUvMYGOqUB4n8YSOX%2Fa%2FQ8468VlFwSehCA%2FZWwrnDLpFxIPB%2Bb2Ur5%2FrDG8kRrgC638f67kMfYKIhvFMg7XL%2BMxrfCdY%2BROIKBKWt1DOsfRUCnjiwzq3nPvHgd%2F8xMtjqlW0H31VLOfS431ABENKLueRI9Y6hzM574oyfXRTnE5tG9fochUd3AUtS7svVs9MUEX66L11vDkPCzZb7bTaiF2OIbbPsprc6Clc6b&X-Amz-Signature=42f2598f21ccbbee0b184a3f0c23ab912390506ccfb8a0e63df0581895b7ec7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

