---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBOZHJOB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7MlPTL1VSXykEF5ZsYzsfhkEOtL2j61dFOBKDZZ6DMwIgEY3yqOq9yhuHlKigQU9phP39GI1Ky2N9eunVoFXhYXYqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL956DbQOKCu3P5RuCrcAwT5oU3ik9h7tzfx5kYwsQia0w543a30lDVle46YOqGeSVDULGTP3rdmpqqSE9YoJYWhdlcK0RKgQO%2F7YXvF%2FCCkhJCPMP9Cihg2NsDzcu%2B05ZFYKwZqCraRM2DOWUWzMULz3laToAZpDNNAHgkR8M304yqX9PflveeSn9nffGBpd0mO%2Fr9tPJQ4oGPSW2rlFSZJbRTbduLUMKjjZ%2BsibNn%2BkB2VHbCqB9oMMBGnqgOrHdox%2FboJLfzzIl1GP2iG7UNqjSApiePoJnhMNPUMIMUrNT6J9qqwI06SZhUBq7fvf5EjcDVBNfIqkx%2BEqjzprfaf0FqTBFfasmyLJp23z%2BGamoi8lABpgWGVxCRUPbo1P1u9qKVzyGNMsmTaIAy22FK8cwZDl9SSFxpmjGQtBQPsuJB9cH2NtSwBcuO9AHQ39X1eEDFVxPaN%2BtjbVK9wXYBPr2hMrQooTgvruAUmZzOn8HYbZ7ymC04WLlcIiG%2FVwmQ2sOjmSDv9iaS32L6wjOgoWHPF%2Fd8OWcQqeztE3Q2emjZ1547D2VF%2B8C3PpMlT1MN9wswBgMyCCIU1k3WWTWvtVj23JqDjUDFkkGspwyzgQz9zaA85tyinWJB80CWyQqpr8aq52%2B3E4rSAMJbtt8gGOqUBnJljqTg%2F0JtwbKNgrQdf1XukdFT3Fei5K9NS58%2FBt8CvjES87eCKwPIkSnjqwWAyzNgSoHkSY9zcocyggoQEDhrJ7OLk4zJNmrrGHFspbYyKUfD7MkyRtR2C3tFK8UnKjKmKwsKt4uwgTXu1Ja65Gyckk6ATVbDntg8fnHsj0giWa%2FiAuxGvCLXBiNTdY2Hn0xulFDpmPD9YDkoflP5gxRnlH64Q&X-Amz-Signature=386c18ce562b33b46d49fa0baff55e24669479d06bcb4102cf543821ebd832b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

