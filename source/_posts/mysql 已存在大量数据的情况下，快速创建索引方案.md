---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTZWPA5%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDbVnYgEAVMy110K1qUFDpXcuAfaRgJASfT70C%2FncstEAIgA%2Bo4Vy3wdTykW9zCPSiq4tx%2F59sGeLhcGFLGGpRcUiEq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDOMfvGlHBLEoQCfKpSrcAz%2F8mQ7OMohyqvjo%2B5JCBx3JTU3bkBGmi21%2FcqsfT%2B5XSDMiCKr7ySqEGVKkm5hHYpd3T3FmHxotuMDsMC5%2BrdbEn01tfxrICApQ%2Fl1jJffZffI0%2B7kHjtpHxKhu%2BGVVfRzHRBSxowq4gbYLtFB2oqQg7F0pI%2FESiGjhICQ1yQSfSYtHyF%2FSxuHjh3DfnsAN70MV7FrvrHt8vbsDpQlQWSN1Jn9O0ONxDT3uRuWXZqxpfickY%2FLjYqgCF4hv9iyRLVGrMKdbhOKEbEeWHt0N5JIiofAQCCWQUDcJi5rwBZDfIIW47106yetPdYwi7svh0j3oeRgwwnfsqrGE97pu4JzoaR7yqG1MEHcox8olEG2rlGVzrvACHGOh8DNCO36a1HkwT1IyB9rOysF24L3YVg4nWFXy1%2B0NFc36pn48D6BGrQ%2FDh3evPON9Ix4%2B8qyL2kRvykFY%2BqPBZ8dThRxCKYFn3tpHXc2DdFfGFWc3kl8GdRdTeGjzz5wtyui8c4ddGAuemHU5zci4pL1CSIJrqqgjV033DtG8itKk%2B%2B4IDuSJmUS73G%2BB1Yp6QvHykCLskrV%2FjMPdK4XYEe3yQ41H5x7Rt1Wz9sYZNv34%2BobnMZk8G2AGprVW3XHWTgvBMMWF08YGOqUBEiuYfj2Vn06TS8czEUmw%2FdtWoXCBBzVzdxibm6pAlEfntUs3xvAEUS8WJlTfyXMyEGJw%2F9al3vS%2Biopd4%2BIoB4YYPkLXqx9lEWsxzPYAZOjtUQzF%2BZ63K1p594TUI4tZLIEpBcEcro2dA6XLGwG5Vq2trhnc821OmdFhz5l82%2Bv3OT6GJEXYetConFNeLoW3d4fu8vWUpRB1uvSkVSrYZ1jCqZBN&X-Amz-Signature=b47efdd8846a6443be1c4d775e58603468ff86a898d49d79ee6ef694d4d295b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

