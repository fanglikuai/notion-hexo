---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHA3PO6T%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQC28xhc7CJkGALh%2FTzxuJ4tdZwsZ745huGl%2BNOMALq9WwIgSLo8PZzddqSUZpnaFzm4sUrVx2cgKGP6hYToULytY0IqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPv8YkaovwdYUdkpESrcA1%2BxH5iMSLfT8aIDeW8sWBPqRlP5QM7afUw8NrIcCRi9lDQfkI1vtu1wxS4veZsWC6OFGzDjHhbT%2B%2BlprctsAo3QLAgSB0YXiUCSLBGsCnBbbD0puPlUyE4uZf9iHRLdT8SN9XxBRiqH2cBoxuKrfwtZbplObe0VRyvzICXXYZqhQ%2BC980HLQB662TwxaacD76gmyNSg2rVyaC0N4OvCAGsaeG88m6UaxbJ0ClfBTSTIH%2FzvKirYSQ3gppeYAhaeEAeWfNuF9ixE2DdEvRiOzF%2Br2VowecnUQocSj3zCg3ZGhkICOuM2NAxvEp2aInMYNS5v1TrCgmAbDtoaTP5ognq6wOvWwK6P5olR50LY7wUQ0MwrgH63JQtLX0D%2FvFsYtJh8yTeS9QCI7jgoQcCGClizj4W4K2bSN3YtvkjUHbMVI7qLTe1ifkAfo57O1eQvJspMC5Kyyrl8QtSHj2%2BhUwqRXe7mVDVW4kxmHWU3cz0kjl30CvhTIjq3h3B%2BuWE4538gkCSe5Q0Whi%2FMHEKOZkinNol3Kh7wp9TUV%2BGmyJ%2FIfoc%2BJy%2BYLY6NO2erBjqqijUlRKz26SujgpA0h%2BJ4bJc9H8YQY%2BTwJtX3JzgMdfC3XhaJnY6AlL%2BBc9%2B3MILJz8cGOqUBcnirbiJqQRB8qk%2FfJnMp5Mdy5Abpz7YMzYK9Vs%2BrQoDtjXpM3NcLLPQPMIcuiNu8NC3LuACrtbrciqRWVua5ywbcHqJKPy7yYp8a6X5Lq48WJ2XTSg8elEQ5u0nMHkY2BTwQW7VjyG4LusvFp5MOjATkv69SDTxdUH%2FARdueUWpZRRnHscH40LPgG6%2BMLFrYvcvjOnsaOOb6kXDGFlnevE8MWnQi&X-Amz-Signature=d97e1f00efdb9d89819222875d41b6edcd964d95c7e93abf7e3bac3ae6ca48cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

