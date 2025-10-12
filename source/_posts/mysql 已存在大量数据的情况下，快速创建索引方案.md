---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663R7PB55%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIDks87EZcuweVxV7xvtsd%2F8UII%2Brj%2BPecLZcS%2FYel2q1AiEAjtCLBqPWDKnQ5VDMG44t594VoiMVWOh401w0RertUQQq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDBizeTBRO%2BLXlkX05yrcA7T7izi%2FRL0lg5EEC0PYvzZMCcPzTVbdNRO3JuFmRbCeXuiK1X0M5m46upeLMT9mc9cgXkRF78sRWGdAI%2FpB1hQ4yma%2B9N%2FLElTE6XqdUkeBznAUJyrx9HYCYyuBIZrjXl%2Bx51LwDuni5Pi9Jj5%2Fz42ACI9whnjaTlsfZz0Q3XZ%2FUQTEqOApRaae9fhz%2FDUm5Ji5fGTYSvFMEHTQHUhZ5gxcfwkoC7Qsd2KnztvKWXzB4gZc3zwVZDPz67ROnUhXEkSHGV%2Bok42wdC2OsLBkArCygSf6eKKWOiLV09IvGK0aGFu6dU%2Bstt85HfRbpND%2ByY9MTml3%2F4%2B%2BaXXEBziuvvDO76CeqFaYBEqj%2BlBBsqUbyvkS%2BmoweBIsDO3naLx%2FofsJR7geWuCdNIODfUtAwX1vSbyQlhHSFQEjBYjb5RHRDEkXXLnsD5SeMRGhLdwj2MqCJgrzUSzmRz%2B1hgvAzt3WvnK9V0ILr7GUPOiM41S%2F0XJvQ9QyfiMPp80L2S3562qhhiX%2BX9p3eJ5AM692eLBqneWt4GBRMJF%2Bej9kcBCVVa9ogo7l2YHUS2uLk7Np5hEILj0PjQkLv%2FLy0tbRFn6NhAbAWkXxlRhRicYSe2Z%2B3ZglFtsgylVFkpQ3MN2mq8cGOqUBwyZIXH%2FRDQCVG%2B4DsfF3E%2BWvWtwzBmEX8gclL9slLzAro0cdmzBSmnJkiqNqe9u4QRCT8YJuFVB65%2FZd0F8Id0tddiw3M4CacI89woYpiVrgr9G0CrP6e6F7kl2Rkf%2Bs2WNyIPaaP4ej1Wp5aIeZ5Ak3S8i6iqBhpEtMWzSKSg1jtToXdY8fYA7PO7LWJ%2FC5rprH0M1fzLxJUIYMpNNdsdaTmFUG&X-Amz-Signature=fea50d0b805209c5b64535a998f3cc0fa0a9d687a10a8b397b69499878fc463f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

