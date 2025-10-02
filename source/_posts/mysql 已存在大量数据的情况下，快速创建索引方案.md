---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN4VL35N%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDQiGw%2ByZI8RIfl%2BC%2FLwA7K%2F2mnmC0tPkhFhTKaMcPzIAiAuZKv0jVyhY6LiDcCv3XFyTToy%2B47K2%2FOWJHASWSlDjyr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIME%2Fn%2Bpiz8OYhOmtRYKtwDj9RF%2FRwjw%2FY8eqfFRJPB%2BGgJ25vQqEXD2dTMRQ1oIJEYemntDdtL6j8QM3ejQvz7SjKb1NtQRpJ7gduz4BopeRNj0HHtuYxBUkdiD%2BbQKpLZ9YStYldjXet3sICkW37fQfpIwaq58erSjfSIsMtQwct4EYHmLKE8oyjU6wOSLr%2BQBZ%2BRR5dLJwJa4Hcm0r5H5YsIfGtMdY%2FCBv7gEiMn59UC4CPfo6SxufufqW%2BRwXGeoucoqmoaq1Uqjq8nUrmfiyno7fxdxuqTOeQIuBgRmJG2S51dbvVOYUm03AZlQVlWxQ1VF4wG55oRpOd3xZtvzuxK%2Fiqs24C7w7E4J5Mmn7SYa8cKnT5JQAD9c%2Bv1HnG2%2BTIgW%2BCUkE1KMDUR7iGmc%2FsarVwPxAumfJewd%2Ff3VDNuRvYjBeGM0X%2BasRx1P81FDaTexuszaSKic2yOBz5CZGyVxXpD7B8av4UiuKq%2F3gCO9BbqIy0i%2BCC0%2B7QIkczi2vU9%2B2se1Sccd4FMo9fI8w%2F%2Ftuy3vJMteW2rdWOD2vI6yk8pknv3FiGUQg88a7Om5YbUeLJAU8omj9Ab%2BMs9Wbwt%2FJC%2Fg%2BDQV57V%2FbgGntM0XgBWq7g6dsVsJ3avn%2F1a8je9AtiixmzcUrIwtvz3xgY6pgFYjDwJnElGtf0Dy0ix9s96Kwapta1BprPJXLypZR%2BqjD3G%2Fob05dPF7F9i27F8TEeZQcVmUzXUONYhWVDXTXmJShmQH7i2m8SPTv9ddyx9SSfof6TFpvrv9%2BMBfrrLheGud4JGcrvA6xmGmha6q%2F4UC3sRLyQXpoiTIGheh5okuDbSDTp8DkjI2xeYWZXX%2FqX0f41vB%2Blh%2Fsn831nOf1%2B%2BnWw05vgz&X-Amz-Signature=57b22ceba7a8558bdefea9943c1b7df6d85af5643e03bb3b895f836d0553f6a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

