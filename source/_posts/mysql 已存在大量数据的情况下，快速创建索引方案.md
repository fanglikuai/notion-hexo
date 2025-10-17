---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCRYVDC%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvVdKdPslJOdBTWPL6InqlcMQcuXQsD6t3f8s589ePXAiBFbbjO73FV8o12hzIAp55nOWyXnuWfciZgFOWUV%2B795yqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMVkCHh6Mhs7PlLW6AKtwDh7h74ATpYXHq%2Bbs2%2BhqWvJAccz%2FhDCkMtxWnPEXfSzmCaCrtuiSf%2FDRgCED86Uq2XJba6NgUFF1XKHPT5GZpBo2l7XFxLXrWnskTCCPt72gF2DQyyCyhJ9XlDhZ3tCNB3JF0fjftQarVIMFK1qHlmblfzsLrI1B5g%2F6kgjzp6qR7SOk307rEguYg16TjxTNACjfZpIp5z5UkXRB3TIQuU0PU8UQBFbbk%2Fph%2FSHcPEhHBZiRmdzbrmaAMKxee1600feqat5xTOb7LK2jAj2aVqcX1AEyjCddwcMYl9RbBebvd0JczaizIPVQVVAV%2FGU0HmIYbU5q6%2BLZ6ybxYw1J3inscYp2du3avsebwIywwbLQN3mxVkxMpRlWUEN1vC4vgVuqmoLwEhj5NeHnXfKBrLCZ0h%2BMg6851FZjTvXTTOHtd%2F4THfEoJrRSyqttqF9RFlnx5TBzIRpGIaiEVFGX2kqDQtBS%2B9kyjeIkO7qPizLIPN%2BpbAsyGN5tvT9bm1ffZkg81Ct8CyOzdit37iw0tnmMJr70E7BpgWH2EeN99Q7IaqquVBGtlPXNWeydvxBMLOBrU3MMBkFjsoyKX5ZDr5JES7FRlurMMv1AOO790nPl52%2FRmWYoS8YWkm4Iwo%2FvFxwY6pgG9ZRYI23q2ks1Edv31kU5FFyWJXoBQASAngp4sW61vN0rhlvaYfvPoUBhg7ydFV0jPi0VyBiEJUopaJtQCVg2NecvEMKLCLp81x%2BBK3rxraFmxh3fvEZFHSgASNnQxI3jO1xsiB0PFNgvAYPWA3Pf3MAbk%2FgSrLhtbo%2FL4UglWHJs9LWaQCIo6GIQJ%2FTJ%2BlMrrGD9ItwMvl6UxYpk%2FBgrM4hnq8%2BEG&X-Amz-Signature=5df540f199a065a9cd6efd3cf1d95fdf841b771a954d7a1430e4e9a42ce6677d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

