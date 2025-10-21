---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRX2IQVU%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJIMEYCIQCAqsON28iguTAZwTI62oRJp32oWltzNz6eViKLw994rwIhALMlmlKK8qPlxqPTMhFMlmZK9Ugie2w3mHNPwUwLHGs1Kv8DCBoQABoMNjM3NDIzMTgzODA1IgzgMLgh4b0L9K%2BlpZ0q3ANlIjeu5nuq6Vl3MgD7qfHejnOopQ%2BZy0cY6eHOhY7bqLaU1%2FxbzccJ7nnW%2FVjw%2FTsNAQxC%2BdzLsK3EgH4cMiNBxn5dQwfvp5lRql40mdwTydxwz1WNX2rybsYjPImD46qLZsovDOVZPf1exr41iJ6aQqWLvzi0j4fBBw5as%2B70zUP2EYLtTSAq6hu8FRcKaqy4ti7pf4zixU4udYXyvauuqudy1Mbmvk4LzWGJ4LkVjA%2Bbjhbe6l0epAz6e00IFF4rE7PSJ2i8gunQjO7AUwbYA6R0T7j9pfUp2AmysVAYzGy2liQe2U1xBOvK4JR3faQzxWOiOGgMX6LWBRbblNjqVObUZnoh66GFwvPjGtRASvSeUSj6SOXPCy6x2BGc%2F4SOza320zTsNI4Ixx%2FTz4c%2FieVn7b2skbCZIM7%2FX8NqHpAJI%2FV2r%2BBAE9LdxTBQpwHhUXCfJ1AmpF8QohpuE73Gqu%2FsBvFObZ2yxceba8dpjUYr80N%2BpFTKT4oZMaTfstyXUbWWnrY5SOeuFS9pjJiWSC5Dp9Uv19y30oL3lMkrsjllUFM%2Bkk4x4Zq9Wm3B%2FfHnJCmjjIDbob0NdvToqMTHRLB64Bepe275HFK9CtodvYIQbAOOPMZOmHFP3TDj897HBjqkAS%2B1BspavBnClnJ%2BMq4OwXZ%2FPC5b8Nx5aNSlzoa4WoS9OGDv53QQxVEG6Cggqm5sjRwv21uROrg4hD2qd0semKJZimQeYjnNEKBOUKwAv0yw4OXcMJLABSB6xGTgBaCa8T8iH8tfdQAXlRQspVxWNJSuO%2BjcY3Oy7RIy3D62G3qxoHqoKles1uVcslGZNgjGIVnH8%2BxTaRfDB6UG7wZXdqnu46gT&X-Amz-Signature=5d5b01f1ba7ea867de6a070639e275a2447fa8814fef7a110063a03cc5e37183&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

