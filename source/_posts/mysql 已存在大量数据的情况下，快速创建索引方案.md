---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCXT32D%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcyWEh9MWVLOnRPDrM9UU%2BhY%2BAY%2BiM08t9e05KTEL1GQIgGM%2F%2BHut8OjDZf4R1dtBsxzS0a3CO32R26%2BIw6znfY6QqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOeapAQs0y0vZtOcoSrcA3id8eIrH4Q3y9tI9t8LffEdyKin7lJfUMTRmaLKxQridvWdh9WDQo2VqYyIDICsdQU1hM%2F8MIWJ%2BB%2FiTf81gPpncTo%2BJQ431ieQAAQ7VfbY4IjkJ6%2BQG8CLuKejGufP6rEe34Bwd76qMVnaNXoW4IbW9heKrk6tiWI0Vu2QE%2FQGxJxFG8VyWSjwz17Xu9kbD%2FiI%2FENmfaWsy5g9H9lX%2Fq%2FdXsv0di2CE4yzhwoPoxQNZ0S38ZvGr1vD%2Fet0uS8nymwMzZdMj28V34ZhjALtud%2B1DHneNK%2F%2BspCF0yHI5uMZulwRrtJpiTVzLgsSqUEVQDEpBGVIsBZfFZ1BcvuJAtoW2AnogKIDrR8hsHZjkSri9YOMlAhSDB4%2FYDJ%2Bqkl0o7Gksg6cEObTxbzOxJgm4H4Dn3xoHcd8edgr0sbnFyxkUfUcZXSdG3MT7F50E7XFPoJtyW193iiUfjyzMlm8Z80e%2FL8SgIE2PqTKcqXtiC%2FQOiqw%2FBuTY5vXI19pfEFqfBUxeCtI8KTaC7c0Jnslktmx3Sb2058h3BpEM1FNAaHq1mQfwJSgtc%2BvuP7dS6%2BB3crO%2FeRupL0b256txQWHvi7E78N8jhz3sZmasNsA2LYd5JhCLGULLM%2BnIRQNMPuR%2FMcGOqUBbiQAhcy2UEPF6woHoLe6Ro5xF3bh%2FXs%2BS9YSZpXche9T2ET2rjzav5rG6BlN0nLcMfIgOfiQoMYzUXu85luofYKGuRTWXpX%2FuG1JbCSAAN2fSDNTFl6vpIg%2BGLkb%2Ff5Mq40lz73cQUS8c6ahmH4s5qm3PXnmw7bjDX8oywv9giiyzu9OSDI5VUb9jClz2Ubm%2BFc6qemmOY3yWxx%2F9KDA1cg%2BTjuq&X-Amz-Signature=4490903f064ec2d29e16e270681502b6c5d98f0be2b4ea09bdf57f042ddef217&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

