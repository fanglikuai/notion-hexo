---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IOYNLGR%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T150158Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjE8U29xiu7mfofam%2Bpnx9G2NdV7VBapyZQUd3zgggSQIhAPy4wKCqt9wjS%2BgZgzpCvBWkQcDCrGZvuQRX1gdtDOy2Kv8DCFgQABoMNjM3NDIzMTgzODA1Igx0fQkhRQa0W7TEzpEq3AMC5znRTxAmttIh8Q%2BX57d8GqjCoKw1GvFhKUFRzkad70NjsiPCzfGtBWz8OG5ZIAQnRNvklpxTEIP13UgOdQJTxmqK1hX3yYRkB9dFrZm4aJ3xfTEotuK4BMuwAZaHek6D248%2FVIKcHc12drDW3EskxzWUFECsQh8gh%2F9ZEoqdQpbD5Dyhtle5A65yDIgOod65t3WfiO9B3Zr8hiqiJUNC8Io%2F8341LIFZ7GOyszEtz2VaWzLobkmuUqxvN3dSzCZfgprrnrmhycD%2B53Owq8VKzmWfK1CTPqIKOqVXLPTkZjsiG5oakBhm8zYB7QT3EUpJHDtKHZ%2F4aPp2l0QNK4XYxkbRdUnM4HxS91t%2FEZorU6T7Dd6vSwerMupOTD7OfCqhDKXES9GRnpEHmRz6tpwlJ1ZQXYf6opiAgd6LaaiOu2G09OiI946HfPCJEGMExSJvAeTTIpyNublASVTqFLY2UYOyLXXiVvlPYovxk4HVfUArqXMiWXululRp47zD9QybhmWwYRLX7G0wS59EHhLV0vAo7HnIgOCXTH%2BKvDdN31PtsHCkwWC2hzOYkIQQ%2F80TXl17iTprIoBkRj8XD9tY%2FCk6uUXdnzylPxgNw9kC0%2FVhcbbvX3qeqL01iTCa2pHJBjqkAVQrkFX7n5XBezHMweEDlJ4W0QQjY1uT98eZdCTuLO3cpfRTtmm3MVbumPfnzQhveCX50tpM8Ajpdt3gln7MBI84vOpgTDwZ8dbvIyXxdkqe7RWc0t59vAxJQ6qoyBUTL6a2izeQ3E43bARbl5Txzno%2FlG91b0QnfVdmijn5vqAui5D3cowHTpkgQIzuPZ8vnTWabAL2MaVK1mefD8ZQijkQG9a0&X-Amz-Signature=ce1a4ad667f2647284584f0ab18ec5fc033dd5e279afef9cfbf698c73212ea91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

