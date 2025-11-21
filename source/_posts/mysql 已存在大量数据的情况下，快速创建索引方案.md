---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NSBJOQI%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIGK2Z1mMAvgTwnPQR59O%2Btx0%2BrPPlFoqiTT394y6THWzAiB%2BXIjqOoI%2BkZtwLAtaH13ASA8gYtvzKAv7YJ5GGQ6J0ir%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMpwmocaTB0uLSGB6EKtwDGxucYPZLMmBqKyUyjK4t2%2FOwqJFjxTphkaT7GR2wKa9yoLBPrlbph1rCdjdrqfC0zXyHUo0Bj1lW7z8Xlmvoy0o4S%2FP3BaRr1h01zlLPMoeYFxFfXvwuHnlbLKAFT4xARKfeUxJO7bImRp4IpsgoyblmI3jMw13mAs%2BMqu3H6mFvcYsYTW6Fuz2dZtLlMo2JGyHF0XePXTufR5z%2Bf%2FL1CjveTqqNmrt1ElO7mIsk9cFBsrzBn9sdDqvH7I4a7Wyvej1iaM8KHLSZCPVS9vnZ6I1rkUvpmYNpfGa2nvBxatswWrf5VpJ0iCVqXgIrDc%2F9I%2BS3mrPkxP06bNANWLQ%2FBnGvo0c1ct5aSDpAy26tcJQAoG4VFki2amxqNF8lriECVbADti4%2BM21W4wUpr0l2JEPj9igKYd8yA3xMWM4YjvBsl6vAMC7%2BUS6RbMz66AENlH0MFOCzkLLMYIdNbzVUb68FkmdWFg0lr1wHKxowg4hZifRA9%2FA5NID6OvNPJEY1jmXkrb33RyO8XsUsLUMzQCjhKHcB6LX9SR%2BLeP5a8IeCIIJQuX%2FNQG5ex5DIfAIRa8m%2FmOUuui9yUmPNz47s%2B25SMFFtVifFnMfGM0CI%2F0qLsFGhaxvMZyfu3e4wndT%2FyAY6pgF5vztDSx5e5CANht9vnb3JFEhXT6%2BZJOJkKnsnu5vwfLURnwdjdCNG1aQlglnRNJX9BhhpZGg356bFszPbI8G%2FL0M3ZwZFl3HrzOPHonuA2JTkLg1KyUoCcxIsakm6Gs1zsc0F%2BfDHYR0ajcCYI3WtK9nhVyxFBrI3a%2FdA6GMjgJDi8MrWAjC%2FZtZVq%2FIavNLbhlgE7AtkzCrzmQ7465%2B8CapzFE2y&X-Amz-Signature=ebaf9ab9553a31ce8475e356701db5d222656b1d1a226da02b50803047173a98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

