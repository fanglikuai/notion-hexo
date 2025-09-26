---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE6BVMXV%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T080150Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQCPACzokJfpANSscZoTfEhDXHVCyEepbWv6wKeVVX%2FwLgIgRX%2Fdh50y65yNpvdqViH9c98lx3tXpOcUwLAISTkqZFMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMKdavrYSUZRh8sXNircAwOtT3z4rrgHYY35sfU9b7Z4a9Ej7REZGuDRJbnwYyUOWtyiKsYY6XPFM5nk4kK8YwFq4Q9le7mdtJ1Qc3x0RV5Pvfk1%2Fja8N8JMuEIAEuxv8Sv8VypP8bPkKLellBlL3r9DOwy3pZwlIum5VZqV9O2zOOKysdFt1WQ5kcBS5ZRu0dUOujjKX%2BQCGr%2BiQyMqloigJNDFz824IqFdERHCqvCUg7a9GieINOGU6DagFbTSAr8U%2FhKk4CHQaIbv8hMTUvNbHNgoM5URf6glzi4QUBU%2Bzz1hSIDOHHqigTluNIgqjZva9MjR3V8%2Fn1RSQKOY7ZHfnetGd9jdG5EOQOVStqlLfGrKZbjPyetyGnadi611fWLxoEb1FXx4PMNDUCvCEAv0xRjoshES%2FN7LWMmUo6%2FB%2Bc%2BrbrkVHejQTlfVdnl%2B%2BWcKEZsNdZXxQPLA%2FVKW8V04brvOB1lYEGS1SsiR4gelldHFEx8LoWqWMhFzWAVYlnvTNVCJ41Wlv688OLS2aJr4%2B6INqP45liSwWqwzMSnefzSAdHA10hHtMX1WMUE%2FB4l%2BypgyoT3jBN08ImWe1Jpi4rtA5lIo5S5NA0wIZWTo88oSMH%2FGby1f99wYUfGGC5zT0jY9KXemIBfCMOH%2B2MYGOqUBWVPplpV3fKeFv28H%2FFy7FxFeCU6EiREAMrmyoKSm4E46rW2jYZmX0pUo%2BujhYV2i90JVg6gf%2F7IFUm5UOX4tvxmD%2BqcNbEujdOas6m4sjNUwNl8ccN%2Br0spNC9h8ZB8PUZ8AoXwGVUssY1mlhmBs3%2B9CEEn7g0pGJs4aFBdWJZTAZFHyQo%2B1UWOnS39wrwjPRKWP0h0xqV01ACcVM08tbBHk5mbg&X-Amz-Signature=b88136f7c305f534c162cd2c3289a9452456216391a011137fb201013c414bc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

