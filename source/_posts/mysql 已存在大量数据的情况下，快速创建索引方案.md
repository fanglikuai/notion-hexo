---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKOEURD3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T140112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCXxvqmmLlOdzlbQOQ3iS40qNG25pSEjT7X5FeEvki%2FnQIhAIx293MsNEHbAUMN8%2BbqhPQv07G4Be7u4FGxeuim0ewJKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnaEc9OKwEEEPb9VEq3ANq%2BJObhNLa7g2TbgyeZmRKHedQl%2BkUfDQODy%2BYuYd%2BqYbCCyJ78SMUKyOy7xhb1y5J29CJIGbXcvWNpYKSRz8L%2Bf4yxG%2BUzuK1J4Lar%2F2GVE0ffODeDCXXLi%2BaKy5xxugj421tbYhpvqjbU22gq0ZVcIiSYTwKGbV10AlJc6q1ggx82qpt6Sj6%2FwzOQKmB3nNubctFaLd%2B25pp6b6Ad8R%2FjdyMZZFvv9wanaxU9D%2Bz9r5%2F1QIGYm2aKYLzoPk7ECi9t%2FMdckNnfJRUmPMHlptBeBRudsutrofRnf%2BxG3ProGdvSwkQI4wMT6yqhCYpnkMNGTcXsYPeibgwvt6%2BDGZd%2FQeI6TyN3OVJhVyuJZWnzHFkb%2BDJ4m836u5DqoiJGeEdtYUGZ0pKHL%2Ff6CrgcT8d2nlXh3JINU84Do7EqPdCi3T2RkG4X8Cp%2F9gLFUWLanab86OQXhqtxb2H6dOkElrbSxmbvQtxbHc1E%2FiyID9cE7foft1gf7CxHqGD2awsi%2Buo%2BvorzcbCD18CxJW4fWo7Wqsld8U3QborwVQ2yNe0zVKYRRQ1tUlBDIKteb5Gpd0loafnhcI921NhvJbZnBeva0nXt4jR3MrQHsjkNen4LjESp5AwtL9tfZekuDCs2Y3IBjqkAY3NWIgIHJbCtjkfreOHM52NRoRMxteYTxPRrseEF1YJXunCzkrFdh5zjFcuWSzyS6%2BIeKM2Uisn2SjcCK2S07bGntrqvdDewGJbOSDF1KafKpdifdldxSeCIs%2FQ%2F%2BRSwyErkGVeifayD3f11MKb4wWsa6LG46IGN0xlMESvTqkC9Qj%2FJhgMo6VDq0tcdl24VFiESfoB1sxRMgfA6s9XXi55STdN&X-Amz-Signature=7925376f25f28f6f77105e326aefc3051c32e5d360c406bddbe7f66c78645ab6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

