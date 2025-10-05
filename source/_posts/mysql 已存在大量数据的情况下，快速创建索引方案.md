---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HLV5R6H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFWXXe54np6Tp%2FNPjDba1lour1Gj%2FN8ruV2tNbX4MchdAiAz9QFGQnXOJbDMxqC1tUXipMcSKzeGNom9DrFGY0IeLSr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMch6mDaOdgRr%2BCAidKtwD4U3gAIoJ6neRoLZUNaPgadYf60jUBJmxrrmlLj3qaGgSkxctkitJ5S52FLvcKe6MbuAlsOqE7tMaYDIT3J0w%2BxUtkx8p35SKoM5SZBn3nQv9etWhoNfF3dxBzpfTQBB8jY7DGzyyBp%2FQcchamG4zfTAQS5Hla7TcQDag3RlvS9fUApmGGlaQRtSc1XIQoue4xjgdiiKC2WdXfsDVF2XkXAUgKTec0jkiFj9aaAp7qwKXMUeNUNyBY7GRsB%2BPSeRyZ0nUnvUE5lmT680%2BZNefAABz72%2F3dqTo%2F03ftAl1pNzb%2BojykVYzsizMa3PqK7gFIddVjTl99zvrAIehgXheYm4GlsrdaVrOBh52gdzdGDjUWBpr3qrXTZOOQfRBuUSZ9kgMLBw72crKTVe370Nr0HdMH4oCOPCaSgsyrTIY8D7IhpvoEmjVGpTGwFLD8L8E9t1WjiUV2kIKLh6YZKxJL2SGoNiBkkd7DhYzI5iLS243jykGqzf8a%2BkZ8sgzybcOY681NDr3f0b5WJEXxQ0%2Blo%2BTxc9hOEIVKNT3zi58tn3GiufHvYRKlwObODI92Yji%2FDxa5%2BkxlJgOJoWEKsckNuYoSwf4c5Xp02r6S4THNIoJA6d5v5J00Q3%2BYtEw66CJxwY6pgE6Igub4NRRMfrRVVHkejevFrFQDSD5b4SxRgTiBTwF09EY8W%2FwXGLjZQ2LLZ7wCQ%2FgDauNFMiWCymPSNGQi6KXCYIPjvVc6bE1%2FWV%2BY3ats1XUGVE67T%2FX8vYP1TnHw8Q2sB3NaMaPz%2Fao9u%2FH1yONU7IyZfUdGjc0EbdOCsx6Rt3PBg68khb7%2F5rdFkt5739hrrSsRgnL%2BD7jJOXtWy8Cd9BuupgP&X-Amz-Signature=e7d5bb478211e44a0d08fecde4a91f249d2142484f6cd76e9dafb37911540c08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

