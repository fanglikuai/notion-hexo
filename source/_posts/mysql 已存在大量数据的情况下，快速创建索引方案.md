---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS5JKU7D%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T080101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCT78bgqLO0YywnyagS1S7xZhQGQvg%2BR1eQcch8vAbnCAIhAI4cxxnXlBdYoz%2FHUknIJHh3ueXzAjsPYF3%2Fp3monT5dKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3P8w4CmLcWxBXbXAq3AP9gVMzZj8J0dGmWcgBzpXy4nDR%2FbrH1c0pVYDz3dP4cfesnJ8Ucd%2BaAmPMtMjx0fpwVYKfTp5gtYkwIgfxjI5qeIppS1aNid1yCKTFWsxTUBkQJRSS1FslF6gOGvbZb4z%2FSzgyeaeebQBfDoobqRzYedJT%2FzNGdOLYx1EhjxKk%2BfAsqQ4imrdjE8%2B8QYnvJY1CMfyX9aYIfPdVlq2OzJzPRARX%2B5XfjwGC5B2TofwYjUPC4vsb420wnF%2F%2Biwrnuq%2Bz25wSPXJj0NbvJPl4dR1zZKkjQiZ%2BzGWe8lp1zoL%2FTHZyfUBe3Fsqk%2FC5uUfd69EdXor4bwKq%2FnJkSBdFeFT8cvBPhGqlYSTgX66Cn1GxZMItzJW31vbQikQB1U5ytjgyGgzj1gAFpe%2B3PrCacCTh0iwjq7lqW88M20kosGteiKe2cFCI%2F%2FuMalKJJT4OuatvAAj3TIQfShcwy8I2SayI1VVzSQDml8AjJgGCYsAvdR%2F1pEh9uGznO9tCymC2JyEPOziWBBH4JF6yhQCT5%2Fp8LvgFTFr1kvc3gvlGPtitj9phwhRUWQ%2BPxfmpHGbqmaj1v0LhdjSTfFrAiGJTQmNIGBHpDrWXjdKCuP1I1Klnt19ePG5WKurEpQU3QjC%2B%2B7XIBjqkAbaGMg%2FYU11lgEkW7ADjmZgNTDX7k3WNVK9aY5vEwdpRJI%2By7DUX1gMgvpbacIQ0vkv7g3fdUb8yd2cci9hCm0O6ElkOPHC3AKOhFdV99dfBQnpZyzLKNhT%2FtGHZlHvQnqwrad1YT5Io%2FVztfdS1VjSkTKXRRuixC2ZHthsQrp91w%2BkUTt6OJGHyJXlGuMfZvVi2ew5msigFWUKI0hpHLoe3k3WF&X-Amz-Signature=ce9181f4558dd9f36b992c15cd93ef6dfd3fd4798e6c8952e2bfa67ce0c68de9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

