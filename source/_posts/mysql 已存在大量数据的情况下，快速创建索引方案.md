---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTMVQHCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqLWT7hdRtmSckIphqcavrS7Pc8aoEylypduG2AW7MJgIgTSqYBVmNnk0emdd20yGN4T4X6NLEGAkcx4MuMBv1Zpwq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDLjhhSng0qfcib4tYyrcAwlqfSz4uvEHGEa0AwcCrJhNEBjuP%2FvAlFjZv5Zhm32k2mVtJmvcnvyRVJW8FbeQ%2BkwBNoi6dhtu6U%2FV010NReJ1cacpwEGIX9IJyuJCOfzReIYpg0wuAmZ5nqt65IItIih0%2FBGz8WP8wb0BY8sRfTFbO4Sh040N1w3X2P2%2F9n7%2F%2FyEwSbrLv6108CREaHMHrvoWQ6iU3LU7THkyjw%2B6xNBXlo0UZvMjQbk2czXTYVAI4Dd3gaO7NKAKeLxYl1uAxypDoShY1%2BuQgBMHcxAgKc0lAEKMU%2B6gGx%2BDONsgxt9jRw1LPdQLkk1hziZfIHGNvosCB0E7fjO7V0BZdc0NLxNC5VFNKW4NTf48X31BGPJpienNBF%2FXI%2FppFNiGAXyleofWMxcwjLFd2V0DV29qF68IGLGu30jZsQWYCZzWVcnMS8EMIzzoeFr7IgrBG%2BYqi9iWuSxLVuCJnggy7ashg4wSbStzwXghGcwCcUPTWibkEekcPR6LKuBlaVz7Gg7HL%2F9yiy4KKXiEA0d%2FM%2B9mNcaQAjGwSbYwvztnRp8JtWQxK6N8GNR6GYLAJpjucwHsEqALvdKMJTXcl659rRvwqdtfrK97494Ss4zo01DgkkRTPR4Y85VGtLwc8UVKMOXkj8kGOqUBl4BjCb8LKGp3%2Bp1v5PcREZ3%2B60gKtriR%2Bu4k5vzpQfjCt%2Bo4ioz1Fhiryo8VafMQ%2Fh9FA9XfadCQfq7wW09sd3pVsK68ICzS8vAH9xpwFT5f3xocQbRkh2IKfAS3Gw2sWMWGedq08myEG9Nh7SjjSDI3tebwm%2BHH9cBiOSvhHqwOqBEF84fp8hHRyxKu9EILubBCYHjRhh7UIOikPX8ZB4MRTbjr&X-Amz-Signature=804b5e2a907d9a406e58bf5b1701031c0e9206cf16ba3fbca0a699a84944e789&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

