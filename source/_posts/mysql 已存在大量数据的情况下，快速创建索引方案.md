---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZZI2FBJ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDdN%2BWGpRFAx2i8Ct%2BOC8W33ScvfM3pjzxAEKzWU6kZpwIgMuOpQdVJP8Aizci6h%2BbhLU65sGomv2YjUGoHds%2F7Q04q%2FwMIARAAGgw2Mzc0MjMxODM4MDUiDG3C5R52zRvyzOMQsyrcA%2FF2Q3AMsE87%2FW2o4j3UxGJyFLaXKlm%2FIw5%2BvaqCRgshXjMX1lA37jr3ptfMsIOa0tjX%2FSSyR%2FJ0hKsLZfbI6lpHHvfuezgU1dZm0EpibnK%2FLbwPG6hwwEq9%2BLWvQ%2B8p3oPbmN0AXpsXvW9dSL1KPEwR5N5ag7yNHxIP2fQ26ghbRco9sy67TBpcpLngFDyYsIFnJjtzxj5g3VApTvboRV3W7ogoB367KAR%2B%2B0Y%2BKEZ2P1ksQaaJmj3HMFA%2FTAv8xO%2BK3E%2BtRSsH6o3LQP2BbfmyvnIvksIXJ0TFydJgDWIMT9nznyWRXMOyWHC1U9SIt8DNAl5Bsd83LWgZrETijn85pmFP5bI57Wu4DO8agrh6xou1hJ%2FLlzlfCQlKo8sQvxBJO0tM5qCW2IL6dWz4fcw%2BAZ8vQlFcjeepE%2BHYvyNw2eo1TDNd0e3%2BnoHXumTGbxmaSwMbkB6xGBtV2fQFM8uRJY2G9mQLo1FlSeHvE%2BkcK2nFCyechIN1J3zeBEUa4C%2Beni2Iu0i%2FHlDpSzN2eTykLF%2BshcCDvM916a5Pi0qeZoJEq1CvqgHcQoZQuMVCDUbRc4biZr6xKR8ikQt8p5g%2BM0U%2BjLO%2B3UwMkhksKP2KprljJNDlada7LX%2BsMJy3xsgGOqUBcgx1WAS9YtA8TTj6Q%2FOpz64cUJoorU3FbnRPWz6r0mUkHXO42OELrjjEq0924%2BGEbS%2B7rUPmyAfiDUxyQTAuEwE294APvMQi018gB1VOZUUqNkHYIAm19mDbqxOxxpjzmo70K47S8vVoPayruayFkNLLEotWGmqhhrxecINYPz80wXb7wU5apz40ICBT2jNiH0BTCUk4FlJKbHAYV1f12e0mRNWu&X-Amz-Signature=0929d76bb1dc4acae77f6f7fe0270ebecf8eccf66775e075cac50817b7dbde30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

