---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667L4ERB5S%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIA9MgX5TbP9g3F5G%2BRD5MXob5lnfu%2BtC1YjkeS30uzNgAiEA7%2B67A3w0IFSAz0TuGAs3xxoN%2F0y9%2BenNddFsZOJeJDsqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFJQT1LEVsms9soSrcA07126UhFBNeMuXa6U05llK40ys%2FrqHPVjIis4OnFfJmMSyPhct3ld0%2B%2BZDDNXeZScad4sLXSYRFTIjvax1YZE7GvXeaSALsYQA%2FYmfFhNiW1su9Mqpt1%2BDyq89AqzTtLiESeB5ZXM9dzT27lgK4f6E9Cp7Qec6xDDzCfpT5HFrn8FccHfPIiQTXFYmjJyAWevnbNiv8MS8UhNZtC8aSOHGjt309czvHI%2Bn7YTk7AfjHkk12IHRENw34WAqz0OWG%2BxrnBfgEa1U99m7kPN7vqSnBO1CW%2BR67LR19ykeaIWlMbpFLMBcjUx1eP9ybZaqawLYIMBkLs0xLlvi0QHEOE7m2M1KPRvQhx71LtyUir7DBFduGeaEyHr588A8eTrP8ZVt2FfESmWecpAtoQjGQwf0g0Rlg4OgYeQfF6qpwLBpnpiDXKLFivRzgSRdE0h7vymEzL7uh%2FppTt2EjadObh43Ojaii3zfOVw%2F2fojdWrG7SCMSh5u6RPzDJ2Mjz71aNv44gmpQunl%2F7PdF6evDr6FLACyFFVi2N2avOZVEK1C1Kg6lqZcmn9UzN0kz1qgx13EeOKOXvEA9jocmC%2BTDDhO6f7uacxgfr1lcs3zEx54ku7Hc85ayEdYQfM0BMKOclMcGOqUBnHFSJpri07EQvCF5TsEp7pbvqu9R%2BWO7gUuWa2iv0jy9y9vuftZZPyaPV3ew1MLaX6X4K3Rvznix6623W8QTHhl0fdySoYcrLJaTILmLJiDOor0YrNqoc7eAJN5wvVNxewocT%2FjnT%2Bkful0yc%2F0Hwm0F7kWQGMIG90x3%2F3B0MzS5TudTYMMIB5Nrs4vd82%2BANKWfbubrXODlcItMIicLAsmZ8YHr&X-Amz-Signature=6d7b6bf502759d8651bad4fe7f0a8730d375ccb330329ccbae575591a4924680&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

