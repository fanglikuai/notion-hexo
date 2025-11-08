---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GEDTNOM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJHMEUCIF8oXOhN1xTeLCv4JD%2BkRIfZzt8zlbJc8oWfFWW1%2BFY2AiEAvDakQ4dSz5viMTHe8rPMoSr%2B%2FoTVuAKh%2FBqChWfBpH8qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bp%2F6TQsyJSGpA0BCrcA3xq%2B7e41aYxILYPCadbLblAc3tO5oZY5%2FrWkfKeOwHPd2qEMjfQ9mPf8qYvtK5iobmvN2mF3DdxHfSBBafQxKowg7vO82Cr57IOlLxN5nAMcz8kv8nuMUbRWE40mWaSBO6LgSKcj4JJN9RU1nbCohpid%2B%2BVK7UvGMyxOLxeK4M%2BiFQgFUPe8fBNudXwJtuXvmdPDhqzcjQ96UQbT7bTnKIv1ukGfkfpkNSfyCVNIx0uxrUX6zTDxvb70G%2BREVom3hzRulIy9NwEJRDTZBXL%2FUSGPSrgraTzOIngURwYZVxOU22K52HcZZZfJ15gsnYsOSNlP1YVgHeLxMQs8QyZLcDY%2Bgjar9aDIriVld576O5zWusidBywQKyQ%2BGtcMwUx%2BqMltnIJoIke5WUGUPOvTtlpKY1OfrF5n0aETAdNfh6cm%2Fvr8n7ti4JVj2JJ7VpfCrO4LoQHL8f6ONTb6h7I27O2b8IZvxCyIN3EOE%2Ft%2F7QANsvYWt1mPCLu5ktymQsqJaayQpxmyGpXGcqhYSfOJtAFyBo7bToi1RM%2FhmHCYyYWIqZWxVCpkSc7O8%2BajKMxFkqSPQxIylWpmKxoNuiZ2LVGCxZPmH2ltNkHRo%2B2CsKPUadrTDKToOb%2BYuSnMKKTu8gGOqUBDE3%2BMopCmghHtn88XSMAHnhPlR%2FD9MQFHiZrdhxGjR9B7zpn2BN9qloKIfRdEbHym1UvA5s%2BILib11przRUngyfsO1h8nk%2BgeJMg81qAY6nNYyGGLUAbpAVlo4ZOvyCTNPXwOkzQG5yov1ods%2BpPNy9MF8y1oE0y8ffscjMllZiCV0uPQrD%2F9T0xfQuT96Q7YYTf3gpfekhEY8wfzBbWY3gNiMPN&X-Amz-Signature=ca42a785b42c36019976cb0ed904d694b72b3040c99323ddee8da02c3eb639de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

