---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYB7VECC%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDq2z4cWVqQ6y%2Bzk%2B83Xw3aZHZaRVHnijbBdYBxoTXLcgIhAOrWi6zd3FFY2Hf7Jyo8GSjxc6E1ScQMuTvFcUXUPf5UKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp7CeMdKLXo9kizfUq3AOeS5YF1oinqU%2BZtLs33kNBmPhFfdUkwZJ7MRGppPasYOIVVkr41seqyHmgyZlQAbDikOw7T0floWKrfZypsUVZNVT0aqqUQsvMq9UiV4FNRXmdNtBTRFTUa6Z51bmAfc7%2BaVFSeYQduQH7gL4nD39qKVAWYwkMzlVH3UN2RnlToyerRjMF4nWu7m2FgZfx3TiLxt8YBwWSTLVbAZy%2F%2BPJzRLt0rGkGE6iuiAvEvW1MS2Z7U3M7YxS%2BzE%2Bm8AbzN2px0iGDDvUiLX2Yk8xFjb5P3sHmt7%2Fx1jBbJ%2BnY%2BIqOXpIKDsCjIcyek5Uamnq8tC8IUI3t%2F1afbW5NTC9wibC3XLpmBy3IIyiagem35Ug%2BLyg32koSryzG3SOHSEOIs%2Fke8LNqH2Vd5Fsiiu750K2%2BFcevTTK83OMG0PWn%2FXTzyPmPfT1gphEM2E7qFUPL%2FWpwA2DbqYKpENnAGOGtKYd0Y2LnO5JyCYburfpUd1RRM%2FEshQrzd9TDL%2FCYn2cn4mEPfL0O2StijSgMRa3kFI3I43m3OaA4hE2N%2FMttBrTkTdXi%2FBNs%2BJz4KG5LLacZQ%2FJzwdOICzSA0oaTdgeBUuuoHi4fR6ZKGAXSwcUA%2Fgpytk7IMo%2FOayzDlMfbhDChhYbIBjqkAbNzxn%2BGo7dJYHI5vOXdj6gpx7bbYhjtTf2SkQMrZnj7ESwvAXECcuAtoPnmsabaRof7gCggNZFiEcRFd%2B5KxMAq%2BihzThSpg7mDqZR4CE8bC78GVCpK%2FUrwidUvbmL73O8I2PMHBcM%2BPw40ampJ0TOoEk%2FoJ3J3eUx%2B7YMSyEcy745LWOrD2ARQZl%2BsOdlfYKymrzCf3cT5HurlhZgd0EEvVNKI&X-Amz-Signature=4245758ea8ec4f7e050d11da151fb1f8dcd0a4f92f51808ba9a9336ffa6a9be1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

