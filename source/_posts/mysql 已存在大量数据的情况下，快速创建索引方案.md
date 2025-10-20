---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U36DDQW6%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIH9pxv%2FVqRkKWNL7%2B7CjUBNsLAdnILHePfo4gsKJuK9pAiEAoTpZpRG4SNw4h9cDQiw5aThfRS6%2FnJ7ICm0JeOzvK2UqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfQQhTp4YhzNfNwaircA8BPtPggnuM8o%2FNgQgMYKmJd8zE1%2BH49%2FOy%2Fv%2BF65YyWP29bEoK5wDxnSCwYP3scln29uWU4RByrimX5vDoUB2Hc3puUThXYHEPthcC8CspSmBx7uK0bZAobCCIV3j0ZgBm5C1FdXzjovqZ%2BIoow2FJpf%2FO40eZrEa84zzvLvElS89OwOsaANounzPbLRTNQwHiBSsri0Ze3E3bRRv9TQgArSX5wEK%2FXVR6XiXrNNdGDh3Upk4XWhIdYNCwDsmjVmPdaopUHKAfJaT0O64QEoZQ24I5kvlNpdyR2SpBDz0MtZlgD6iityyu0ArXublZpSM87gMOq9ETuK1NiyO1iKFO69jY5PSBT2OsZi5rfVlNwTDZrAsT1uTK%2BjHgwcyjd8qFWIuDLbQY1K%2Fd4kGS8VS9UP5FYZlSyuhJvjS5xrkIsLK1RtAMyQkxn1HBM0cUYb32hFB4ujpcGbA3L1uVD6AbK1OqggHb2aZUQ9U5Uv2kNORjE04DqNRMSQXzdXdEoM9hBQ%2FpJF7eaPWJ7rVqjSgCi4nvNW9Ob38cxmAoSu5ysP7G1a4sQqTs%2BSNB5oGwjCAcALMFTUcGdTMewyYfLRF5I56VKWX9YdX8o6VH%2FZMyFYtsur86FtwlEkMKoMMC22ccGOqUB%2FVWNmUuJ4NyuRqgLw%2FtiugYif6eVG%2BYv53MNxnRbOrofkeYDIVgef0lyjHdDL7GiHPnQ7bJTVBA2QbKn7iQEZ1QqluzyXSv51dYyhv6GCvr1ojHVIGPukOWMw%2BL%2B71a%2FdQMhzxD2oQBVUXY35Nk46lrqLJX5DdI5TZmGRq4jOqbAEFeQjKalz%2BENoGFT4scJYvarq1n4WoIfyuNvKlGzaiOzcIPJ&X-Amz-Signature=fcd940fd6cf979e47e5368f0f1aa098f5f7cc962beb86bb6398c2d956bae4acb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

