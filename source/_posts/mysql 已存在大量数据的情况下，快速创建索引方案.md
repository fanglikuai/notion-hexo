---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YF45A4EE%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC73dMk%2BeQWblq4f9GCJCixbNPxt9h09%2BJ3ZQpz%2BunNoAiEAsnNY8vS%2BsxX4FReYziXv8%2F8KpA%2B9GsXGWM8Z4KK90Foq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDCQC6U7qHuuCTc0aoCrcA%2FCTleU87LzZxMGQn7pxR9dNN8r27ox1X9DPoQc4rBvpPysgJsqtH1AqWWgYrAc6XjpSPWTgn%2FUx3QKm%2BhxhMrcxNTW9EsyBsm4e%2BkUwjzCYph0Q0ibnWqcbCYZUw9KIQJqQgbsnzFuZG7%2BddcX3pg8OKdR5V5%2BXqyojhwO%2FXdsfoY%2FKw1mu8EM9%2BoE6i6l8kMgB8antS2b9BFlTVVAmoSuwzTYLEih14cWdjiJ2N99GtdBzeOs6mGlrVHV7ilmdPotRKDkiJ3%2F2DKaOqItf%2FAjXbU014hZUqXbZSkliLbiBU5o1jKFfuyVadRmJlSdpNvhoiRo4%2FyW6PbYRtKHctfcNW%2FrAq%2Bc4DtmdkCY%2Bi91y87tHzkBNNyVJ4%2FyzW5dD6vTAUPpEVUie1kd4vAO9Cn6ytftOe5By8XvXAmWQ15iMsVe6TAsK2G%2FLf%2FMTkGb0JbaSPAJLDjH2khh19TNIPOVTahh%2FcNJVRC%2FO4s7nT3Fgz6BaS9hEYa%2Bsv7jaabjB7%2F4lZ9Fz0CWzDTBeMWpNnVKPOdgVKyca5lF3H%2BU9MXTlXzdQfZg3hUnR%2Byxgm%2FRDeXntHLtcfvdKBiMpfkSrBU4ih9Rc9N2MTnjnxz99lAdOD22T%2BzUDcPFWG%2FSYMKiYuscGOqUBdVHJA1r%2BZDkNhdOiFdF84ipTCk22YK6VVi%2F4%2FZ4sIYGpc53dFZjL%2Fk2uyS9H7y874qwbMgrM6H7sHWZzs5R8TO6QnD6zvnzfu6uYhyMjwPfySHvyGFd4PuAMFgblyT%2BJ5%2BjnRBIjEmT2Xwjc54SS6AhPGg%2BLNEDsfggAXoiJE7Dev%2FHmmMlNlB%2FhPN%2FoFaBavv5eWBPtuj8%2F%2FDTs5iFcLRQMgvSl&X-Amz-Signature=29cc0bcdc426af6e4ebc1e5f69de07a4eacfd48445ddfd79c00ee403d509338e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

