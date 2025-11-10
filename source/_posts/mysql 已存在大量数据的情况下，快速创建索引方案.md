---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNPCC74A%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIFpqWx1QMX%2FKxfPE8msVzzgfREExEQZ8QLE7Q8hM8DR8AiEA8FuAHjO7u1P0UML%2FgRWthNZ63lPm2n7W3xvC7LgTeYMq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDATW8e2Xj59h7zk4nircA2Yub6%2FDeww1MvwRAxxJvGRF%2F6hD8aR%2F9qBPYxyl%2FxAv2cHgYiubGXy1mfLG5xq97lafqC88KSfmI3ailg%2FJ2cjCzJoEEp3BT8bjb6SF2WU6eta5f7CL4cIu8OPxTEU59Tp9wgWe8Wv%2B8%2FFufLAiVq2L%2FZkFQ%2B3R963kpnCamJGj01%2F6xb8sZwH8EqnH5MRGgB93uAmyCmjUeVAfBUrxJShUQRTgl1qi7z5LXY52Z5upM08zG9qN8C%2F8k32d6fQiTToSrj07bbkOosp4j3jbL88rRMF9WNlOUVRoCSnuJAc1df2LXy9R0fiLAzmtb%2B33SdO%2FQSqsNFCEFnckw%2Bsb49us9TYH0QXC4EKmimKAXeBkYmXXk8gd3cX9oHwc6rl3unGvd0lFnPlCzGQBFaDIFeiZJLPUaP7AjZvDo46Ody2p6RP9q6S%2FDs7J5yIRyT8%2FYZQdCG4VPZrMj%2Fh1i3sdI9mcWHFkRv%2BHZbr%2FURJ%2BdAcfVQvtj6IbfEVHgf1fgCrV8CaNo9pE8ysaw%2FT0%2FVyU5aPZpCEay3ARs612itmwruM71gSOViejV0AqlhwW9CN68A8iK5DR7erzPYAe2u0jR7MBChfR1rAmreAl9mNsV%2BQwpgvqguA3Li6YncGHMP2YxsgGOqUBareyLUKrY1JR2%2BKL5%2FEbSnNGsnma8DKl3%2FHX1wR6471epWpQmxDLDa5v68YxDXZmeIs0S0Q%2Fl5Gewn8wkLh2JqD9mvWn0YXzfBSZl5%2FSi%2BX5YF9l6XwA9B8hOE%2FQDkE9nj5j1Nun5S9L5C03aPffPmwAzX%2FG5NBKX8lKqWz80HDV%2B3HFKzC19aGA5RLG3Gk%2Fd9g51w8Zwl2LwIz8Ju3W5YbyDo3o&X-Amz-Signature=a4e3a849d924a9e817aacdbbb074b7179d7d157d758539c77f7dbe4565f93045&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

