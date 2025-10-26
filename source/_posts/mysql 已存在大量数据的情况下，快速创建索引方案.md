---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S354QMPC%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T060059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDpPpGKEb4hcT1LMn%2Bz01rXuFcK7b1ib8aDVSJP1VrlFQIhAMpakNBm1VGiqBgH%2Fu2Z8bMwwf8MUjSxgdo%2FtJ2rseluKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyfl89CLXD8rvSAHQgq3AMBG76FF6Qu%2BJ09E9xK%2Bzv1dh2XDjTwnM55PBe3yLFiTiodQe2ekdrHL2cHSR196oHb4R3bqNeUN5zgdUe1LmD7%2BKw8j5wf31JU4X5CzzeDIeLUpsig9szH3EVgOWzZGhNkG2yd6%2FwQWcbjNMYGGTIRs9WBawq4fzqg96H8KxWBgY7LKK%2Fq4rY49o1kERMwNjaNIZCqaaejei%2FLaPf49OrDc8kyivGaiGfBYp%2B1KCph95o38eN3QtSK9FN9KaHblPOazPlOehMYgV96%2BNjHzxRp6MweQHeAGEle8KfJeqdCQUabLNLwuTpewagRwZaIBMy%2BjK%2BA9vp%2BBre7nkSRrORJLlZn2y6%2B1a7Qt18F2tWppqp5zboF5Vkooz0v2EgxY%2FabmdJS9RdCqpx8oubup1HhXooQvQflVAW1tP8vSVWPangLAPiN4rhasqyL4XmQvokPpEpTZTtCbUMdT6vC2P9cjM2xZ4AivvrSmY1nJRBooOOunwrWi3pxV0Grk4hIhO17GLRXtGtx3mE7%2Fk%2FctgeNirK0sODtDQxaDkz%2Fee9S3dpntPH37ViRw3IQo15b1fME%2BCzdvylMmyLiBWuA22XVgueHlD7PO2kxaVjfScbwHDCx1THGsO7tvfDjeDDX7%2FXHBjqkAYKbzm5c0g3BHkFz%2BzqagGFvn5VuJUcgqLMgeGSJbvkv2rnQNgKZXzRORlox%2F7GKxw2dSdjBTtP1WrDerMilUk2Zw3akYHj6p7AmCxdaLFWgUml2b6YeYX2w9PAyqWsQhZx%2BZJarqpwp5lnYulB6zPCsy%2Fk6lDHx9AFimR7S8MPY4o3tVoVz2EtOdiyemNuwg0gsLWLLckdUFT7gXX2yFNQOwPDA&X-Amz-Signature=b08f43c0556b31d44c6b3a0e5b37b8e1a9554a32739c0d06d0cc7e9b2932266f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

