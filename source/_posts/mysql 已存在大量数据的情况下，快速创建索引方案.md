---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CSWSQ42%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCYJfJ9MI8FNZRTga0NPceJ9%2FQcGfbxR10N9KIziPhBaAIhAOadWUhUS0u0dbtfIhjd2c2jVFQwRbwCghXV84Ob%2BlSoKv8DCFYQABoMNjM3NDIzMTgzODA1Igz3l3SglkZU2k%2F3Wn4q3ANcYAAT1rgZeKbJ3moeYYdwb43wzRJFug1f9lEg8gvwUVz%2Fx8SUPV6B1bBs%2Fz4NVvFHbfDFLCXuB475KA0fs0%2FtVntsle331Qp4iUgDEXVjwxPyBkNJ3%2FTQsYiV7qDiQ15Dd%2FhKP6G8dneTWDfEtStYqTks7nRgsWjkPI5L9%2FjiH5Po7TbOAbXrGKesL%2BwX0oLUIbAg4QcB%2BKTfgvl4%2Frvjn7AIOMyxwTFlXx2EQ7nqdcqf4ygkNun5KhSLuHc%2FtqUto2M21cfuQOb3JxfT8JTSTviD92AK2Hkmkh%2FYZGUkPnFuMRtSnuwzTD4hwEa9M058vVfQqDQuTZ%2FThEPvQk%2BE0z7jOiW0CW%2FH4D6r3%2FdsIRzCojtVUsdCLKuMnvU%2Bt11yy%2FNEGo9TSnObHq5LhXFGVQURu311Wn%2BZkYTr8OslLRz4VU%2FaJwbEFTP4v7Ns3EY57HXLSQyjGl84dauwo7fWDGvKRROWzDvuLKPCN97C%2FEM9LxQibB9ERXouk9gPBHEnzackII67I6XJcz43gvL5FBNTCed7X%2BH2J9pcE60NfD6ewCOMptfLGFXzLP9RjbtORqxAoXUlrl2zHcdcExtZ0WrWgPc2NlBtqw4L6N17K2tX6DlUgRzFq8zDJzCajOzHBjqkAUvYROswabIwla%2F6T6SyuHebcR2f3eO8GHDSKuNkvqbmUb%2F%2B9aBm09R30LohjHaHl0PfB0mVyghJbw%2F0SwWTyafan6FwPsnrBtoG4F7I42U0L7Lhc40I78y9epEJts1Av1yno5tTfC6EIBhHSWDa1TbkS9KPw49OwRj6DcVSKw1ICpFFoIaeVB2LJG6v00AyduCdg4%2FkTDqp9xrDY5%2FcFsLpwm7v&X-Amz-Signature=404cb2433fb6aebb624771669c90498814c508c2316dd4fbcdd45daba0d7bdef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

