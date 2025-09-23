---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTYF7MVM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFw7EK52jmctIC0DXq3VOigSMcce3Qd3XlLguRZ51e3NAiBku8ziz7rBFDpwSH7Sr%2B8QChwswKpszbUqlN793sVKvCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMK8fN4%2BAozVzCMNvtKtwDFVg0fFMV%2FMNYRdnvdU5M6A6SBRQZ9cPoJIDaF%2BxUdiQtyOMqf70Z2WA1p77CLUhCau5vnO9PLdUsK5%2BIfYbsS2YTUTFV%2FqzBqN8SQeRax1uYr9AsF5AgD%2BIf9RcN%2F6saN7vfZPPtoTayY5nUzbaF7Qn8mb2skzGAs8QEJh0%2FNtLXaY4teSiRY3qPWHujrIZCPFvnmdlD2aVO3sJnD6GGCnsPSrqp0GZXJ2k%2FtIePC%2F1mnj01zUJPNVRINelrUq3UikiPMS2IY7Lh3AjM3lo3e5pDxozgwmVcQJ43wsPkesDU6pQ7fOx2XP32L0QJpwWFPT%2FPsblvOG6%2FWVRPG7QuQYEfOoxkjtg46lpG0ehBqjRQswbMRmD8e5YudCOHhFI2Syp%2FeVYTYE%2FpDEg8G9g2lZPnYbKR3b62Y%2BoHPjEk7HRZPyH%2F4GOd0NCVpOv7vJmqibdAZ9h8ENitfs85yVtrGMqwR4lMnpZqYLXlZ5Mvp20nQw49KB8Z2vDl%2B4oOFvRaADZ51XZ3orGMnSM7WwpVsTmurhaMPhWHc1uE702cgxlsUi9hwivMVb39NmgrLne86aznlz%2F238EUNjkVQH8NcxHxBfXK4xJysc8aoDB%2Ff7x5ynYO77gphnnqKxkw2vzKxgY6pgGtQcbhCXmATf2Ccm1kBn3ufq4WxeaQNmLX71Z11hk1hLE1VjB67U6GgI68WKzQ1UrJRFJI%2BIpeVxxwcrFSAGRHra7JU8yEEE%2BlfkiQ3GrRNVWRmXKwfKYLnE%2FTVsvWrjsEELVVrySKZ04QbD6VLpZC6uWgnoQk35CnXPaTbxItwsWsmlnt42oRrPnI6wa1uyJu3tD6DzPsH%2Bhzp0bt%2BSHS33AQ7DX6&X-Amz-Signature=8342d9b20fed42d106a0a62112ba22c29399a1890092a167929810b82dd1190c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

