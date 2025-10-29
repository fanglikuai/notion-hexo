---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QRPL7WB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T150157Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQCQA5PC0ZYiEVIl%2FR94GLchD6r5evIIQv1rrMi43CshCgIhAND7hKiX9bA24BM0XAYBGZMRq%2B7A4CAp2F5TfT3klhQ2KogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxs3knrPdfFcrFZUqUq3AN1kIXzMSmyjb46%2Fl9JAj96t2iJJAr2Y8hMMrc4yvDy%2BFjkPoJlqkXOKUOvgNQKWOl1OwX%2ByPR1hV7dlWEL32ceknMVxRppNcx7SzG%2BTVyOEQKi%2BEB9iMONg3KsWrKDmek99IvDyz%2BI%2BLNTmKvhP6ZNheRhYgqfJOx%2FiI055cKeDEKdrD%2BlnqngIZTo%2FgpufvNItLbwBka6D%2FcBRY3cGhivOSRyutYQYAb7GjJ849wXi2V4kos%2FPlCZhiug0ubpz%2B0%2BcfY5%2FkwYyel8zjMdx2y7C2hNHrSOzKD1jRgn8v%2FWjA9odiVxahcMW76MRiemrSoRQMIC6zJl31shfSM4jCDwnUAEj%2BgAKL1eHivNV58uWIbAk4Yvq%2BVd4Jo7U5743ADGl6sZLGRK0IF8sJwikBz4hRTbqfbGawz01pS%2Ffohar5gB4lN1oDKiSaXw2r0C0oMLwaJToUduSkHsNuzjtuGwOxmoN3cjqPCD3RYbQ5lYpD2Tqauy51R9V29z3eFXxA%2BjUrUR61DAm0f0ZmhU22CXy8unZKpqGg%2BCtS1kjO%2B9cYNhmhTqB%2F8NiXDnw0DjJh6C1j3zPVjZhYTed2DgysnEO%2Bq5Zgtdlp6kifFDtDX5SpNjGIMS9pKAFGYG2DCE1IjIBjqkAVf00EHXCyiuukw%2FAT3BrbN%2BMYLa8SPLZ1CcuORe4cHr484x%2FQehBu2HT3OcZbADorAbwzthB6R3fvae4f3ggJW3JUAWuVlLg5QxGo09MGL04B%2BHCZe0Bzis412b5X6ZOu1upulG9SzVz8PAPoRKrspl9o5G7jjmoxUOmf3oI0gvFC5bQAnAY3tBmMlvbLoix47txaOvzZoqJE2SF7My4bBfOnXi&X-Amz-Signature=b6c4a282a43f824509d483995950c73632d3879814b28c4cc15e85410bbcc719&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

