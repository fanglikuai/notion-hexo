---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTA2BVH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJGMEQCID2E%2BPGBp4Ltu0upM6Dc4%2BFTPZY4njMITtaYBtjh7VbmAiAh3wPG0UcJpIelp0YAiLGxT2z57s2YUUzg%2B%2BNpZKRn2SqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCbayZkj0RbWWOX3CKtwDzLSZ2KjmKZrwgTbZjTNUB%2Fts1eWuEx%2F%2BWg9HP%2FWIF4Hf%2FnCOdP3KBr3coUe867zdM7v5mVTATNwYgdEs1uli2IxL9vc46zQYNcHLn4%2BOVonru06OkibYH0qGANKn5MRBCXGwx4BnQlh96zL8kiW45bFbh1uzsbfIvt%2FvkEPqN%2B4A2Os%2B1Au6PlbodvdTVJmsGN3sVV5JlcJrZkJea0yI9LvyaJtSyYaKCMElswcRwi1L%2F6C4ATsb8s7mHEkQQ6posgqiS%2Fve%2Fb1zLnVA828moLKHzI2qU%2FiHUSEbNUwaCoH15GbPc3r7GX1qoNTXfvgDGhK8cqaELh3DQs2My74wBAKgE2Ss%2BVo8aR%2F3o%2FliRpfJIYhzPPOoYHjNBCgWI0RXFu1FNQ8VoYUjV7wXiiB0oDrqF55m6pEe0zR6%2BeTeoW2t16jl%2FpR0csGxjkESwWelU%2Bqgwv3Q7VimjtljSAo1eE3%2BDEzo5zqXaBEgLuwFPfflC0Y4Kl6T4f15l8iiYO2c%2BXAyYEckYU%2FlPHc771uuzV%2BQoGyw6yQh%2FJ65fqAsgcuPWqDn%2FPU7gNcPBYF4P3SEWPT0dGs6TxnxlAhzn4ZDWptwT1Mj5i7%2BssXlJ0vU78jfwC9js5AzOcl7ke4wvqyHyAY6pgHDNltgqBRLwe3r58n93ssyHrUf70vHkW6DighKtVzO%2B9Qwx%2FoRU9OjzV3Lp8wUviLHRyOOzOzp7I38pWBFK7bmSREJc4%2FvedsHF0l3%2FWg1O1nEdJQygzv4IfVKJ81WidGLIVEItHl7q8U9RbnxkR9YBbFlwzmiK2dIIuz4V5IMak36kUC5hEknX3%2FrZeHKue86bXTHrICluUSCUmDlq7szQ1ARIxXa&X-Amz-Signature=9b5c46bd8e94376addb3e04b293b1a86c8f90f45c6ecd9c682650bb58bfcf4cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

