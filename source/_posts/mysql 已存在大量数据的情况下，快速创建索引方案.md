---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZOOCMZRG%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIB%2B%2BIgV24kYAWUGqnOKKlogyHRQm1q9sfTzjcImI5%2FZ3AiEA%2B6UKZmjtl96lIAvEFTAbrI5VCtmj532LJ1ebMK1X4qcqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHpRzLGU5fJrxUybnircA1Mhz%2F7plEiEpjP35YiWG9NkpI%2Ba8HVBvw8MzsfhqZzf18hDB1f7ak1eaeaFGpdDlQjuHSdmxw6RBCYboxtE4c2yaJ%2BAyRQ0diTbe1D9gT1JplQ3VFeIZFxGs0NkLckMetdwhC1hArljkGWyi4mogWezmy%2FGFdV3nLoeXou3fj%2BKGMpDdZBCh20sJOY3zYlVTjqqUcfyerh1l2IkBVDt%2F%2FMUc96rP1inXpMjb1%2BPwnef7tq4nnvIvMFgC9%2Br8RJYoQW9pw%2FSbCy%2F8jgihdTlRq99nqDJ4JwcBRpIxulNiKvgZ%2FvGUHOVQTDvN3pf51m4k2StOFFk7J25bDRoIdZqgsFVjyDagw7gx4CQDxpQ5TOjmwVTCXmY8FEZS6kh8hGPWj0JOnoWDlmidQ3IOrLmLH3noeCYSI%2Ff5gWHKSQkGUMnrrVNQtbBfjn8jq2UM54MU7Cpjl9dGuDMHueI5%2F3hLMJ%2FT51pGnxM%2Fcgcyv3ysiKJgVdvlA093YCXUem86M7xAcCWRBxThXHbYjMD%2Byt%2BlEU9B6z2joDbi2BAPiK68n0GLX2qrDLvE2urk0ZG5lYZ%2BWhgwLQVIRBmRkkrE0ufKZC1HfLMKfGfQLeuLHVUXeeAD44JcJ6Wak5YdBabMInVq8YGOqUBwarwJ8%2BgW0%2FH3sFBcRQO1cUMPKVqLRaYIcBq9tiYQ%2BL24LJpQFd3s4TpUN%2FEYPabzxiZodRAIVoQciSjinzhlYwJFdawVoWSPDu%2BlqBfUvn1deJPBkmDWqYtC7x2lkffc2ppKP3H%2Byzymrri6mlMG5tzHZea53Z5YiNlLjrioGjY7i8AngEuUX8gZKnD32j96jLAntQbb5Ml8x64gJ4gRhk0OKmV&X-Amz-Signature=939057142faf5783e003d1cc7aaafe611ca8262f54e568cce9529b4e46a6941c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

