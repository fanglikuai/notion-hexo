---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SP256FNB%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHSHD%2BLy5yHyJNMiXLiZk6rfdB2j%2FakJQtKa00qaFUzbAiEAwz5LJHRtcfou3sUJQOZPm2v26PN7Di%2BXPis4KzpaMFYqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNZZRZRYRIzhAbtUeyrcA5iTbrR9OvfAcsvjQDy9RvD%2FIIi7rIHS6xe3kc0VhIy4fA%2FG9AUyVKQ3q4TW%2BToqMWTdsfMhh72GWbgrfdmqTqwhBwjl%2B278iMGAZ4w6AbHvjiRo002T4S%2FIWV751EhnbJ9gGscGJ2lMdJYuFGU7agkLy5XgJTZ8trNNxKerLwy5YcAUAJ5%2BgZuBbjCi8ATmTfHN4DMsHDXKOAyMJk1VTvCJXwAyLEh5CC7TbQjUYufh8FDVwZ6RwSUcoVoMuw2J409X0Drm3r6fek5XTfZJ7wzNpQXDMGwyYaoUI8UDD%2FTQ98wwaUGG3gRqgJDNnUHqHRLpNGGSKihtHWb13FcCk1wQzcwCqzOJh3T84zLXEeFCL2hky6SBIv0gh%2BmLyhy9G31IMduk3H%2BPQ2obY%2BHk1p9C1MxG0rSVYl0q90OAM3zLBlp5kRKliwHTfT84d7x0LkxiJtOq5u%2FrII%2BcjDUz8rCGyHONgNEWV%2Fb19V%2BYT8wSZHi%2FGfMDqi%2FwY9HyJJHtx48K0g9Sfh3bTfqFtI6R2%2Bs0nDB5Wwhwy3d3tEIBl4Ry34Eub8KYJ1td3IdmS1X6s3D%2FYYxC6PfFUSNhVMUy3NmSgzaA0zFdtBVRkOymeJh3i1WtXb21MMAc2BiSMPTD3sYGOqUBiCiVVk2bXRYaqGzE%2F1Ya8a5dQyVC70XXVOK5ex33Y4W3v4IWLcrAwDljN9MQkZOTFWQEsinEuB%2FbrWryPLbuhOxZLsi4URWFVF2Ur%2BimAcifnIiVbZSqvLFwtJO4voUOXGCp3NsmyhLeZIQyuDArHmq2j95MMS081iM%2FOfk1yY8ygw3Eq59lwecuF6PcHf%2B2mV5tCp2pfwrNqheV3h4cQZWqoG%2FS&X-Amz-Signature=491bab353c14a142fa9f0c05354efdfaabf161b25074fccc31ffed4b1543b71d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

