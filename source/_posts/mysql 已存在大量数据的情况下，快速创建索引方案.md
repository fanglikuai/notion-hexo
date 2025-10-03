---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CYGS5MI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCCRUFA76Zo7LAt%2FoOTHrj4UW0pkx6W85ZKb9xx7gKSLwIhAOOVIJDRKHeJdS6VqTu02P2lnXT4kkV1C5fcIrgGseyxKv8DCEcQABoMNjM3NDIzMTgzODA1IgxrGhlgiyoPSbmAraAq3AMgRtew3SCttCKgF%2FEPaOBYCxyhXl5axWqKhV4wt252%2BlckXqC3o%2BCL7D0YfpF%2BY2E7uS1o5e2FMVZf%2BhSMiiPBOWzj89TdG4BLF%2FeKj4mAsoHiaXjftBYYECk0uwEBkwVzxKOHFPMb9F0l3Qa6sHxBFQyPEeY7pc%2BMOhp1M3P2nlqdKrwmKjwghiYkYZefq14TpvmvsKUAhVaqGwprEoo7TVMbwHLtn5kDaqqasHDLmK91JGaCyqM1N7s7FdhKCn0jVho0Z51pdfInoFjBt8Za0YQvU9tMnrx%2Bmj9VYlTYwsWiYMHRtt379CyCNdHGKU%2BunyFBlEsmd7WmjfKUWRNAUz72rXtpv0LzaK1qGfQgBJ5IF1aodKC6tAB2u02ShdN7linxV3rjSCI9nBwHB9flpERviZMKd6Bb1vreNMbj6t9b37YwRSF8yQ3xX22TvRKohkbClP28gLkVlUCC1XaSLXupb1Owo%2BCZiqgWjYnDSrkh%2BaKO%2BTxFVjlD92%2F8%2F42wiGh3UuSffoFubidyb6Z3MtBkLiKUMjsUEbDtovh8IT%2BDx6lRWnnk%2BLJSnxqu3%2F2fMwmteC7AyG3plsa7EoRyKuGNaR%2F5vAhWmjR90cYvl7w5SLV7Bwcbg%2BlcXzC%2Bs%2F%2FGBjqkAfYFyQhoK14P%2FE2i%2FcHMXGK%2FyBxjEm0exoEbhLmOtnzghCrW3%2BioZB4I5z%2FP4IfstAVPJpJ6H5EnDHgRJWK2rKdQEKlSPwvYl2tI6FXJY%2BewEI4%2BM2Z9OUV26YP%2FxiADXClEQcQq5ssV8dDv%2FY5AgxR3tD6CooAs2zc4sM%2BWljGPArwckxnJPNg2ZNnMNNLJ%2BXS%2FmFoSqk3wCkyQKe%2FJsevkL7A3&X-Amz-Signature=6c4c5d5ba9fac1b2e639114e4ab9c8f50f1785006b99edeedc3cc261daa76ed4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

