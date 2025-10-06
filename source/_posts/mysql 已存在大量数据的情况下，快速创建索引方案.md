---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QCJQBWM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSoEbi3tBGg%2BdckLTt5Wc7zFJIq3eBe9pCfgCK9L4gSAIgHA0F0yc8r%2F%2FSG%2B5eBD6Ubvy%2BZRLcEaTe5VbttKRYXyAqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQlli%2FII11nKUptJSrcA3kr9mGMqDR5lkfFfUV6EcrL0Q5ticHB5%2FbwEyrQJIqk5KvALapevq6nnh3hYrMCSE1cUlQuUc1Qe6zuW8E5N5yGTYp9b2bCJShM2sSpGo%2FnyjvpRIzZN9LGH%2F0jZgebFlI2BFlmqZl1tbiCPW%2Fb63F4V%2BxdsG75bfAamwuzboa1wh55ynFpJ%2BaXiLnzZPL1r0r0tNe7gd2c0oqRuEC1u9QKnYPhS195KNlm4OQDzKFVWSpxSksKD%2F9%2FVZzXUVphyxDZQnsMa6hkdAKHaHXYMc7Mcz6dRJSjuYMuzAZ4A9WGY76zrVU%2BbnkDM%2FVaW5h0Zry%2FsPciQuWYKUDL6nvIKIvT3vabec%2FhkLhvuxjFfQcqQbwk%2Fbk3fY%2BCs0BXHjOcrDABVyhKItk3GgNEkd4ZyNY4U%2BzaLMK%2BfjUR73iCDEb%2BnoVRjck34OmzJZ2vgUHVu5t2aYNZPkQeD512MTKcZTkwsmB5zerY05oV8RVWWQzcCLxLJlqv27581CpecNXIx7HG6xoCpvS84YTnheaGMfYYnftn4nz0FquDg4X44NuKW8KZZ3WhJmtg7rt%2FcgiFU%2F8rhFGk6Vrj3SU8wpz1LEor%2F2GuUIDMyOq72VDjrh51Q2akHI66QQpJCjEfMMn1kMcGOqUBD8O2mNYY2vMfX0baTduRzO3PWBxcfuNXKgyVXiIsOjt89RiRldVxBuLJ2wUspl8dwKWh3p21Gy4oZKKrysNOTjnRpeTt5ENMor%2BqPyWaCWlEys5N4mBsxGZ7kSV4M%2FpI7zdE1vHPZPmwDdjuBwwYhz4Iq2wvhF6mftlO%2Br4HZ8igkJplrcFmUAV%2BCSFPjMGNDpEVqEVyjzb84T%2Fmmo%2FmkocRh3Q2&X-Amz-Signature=ee8bcfcc79fc8780ef6c5ed5241634837612136437dbc465319125acdbb2e7b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

