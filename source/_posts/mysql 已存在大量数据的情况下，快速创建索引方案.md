---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYHSHUMQ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCITCdOkvOlDKnQ1riifNL4sG3B3%2Fgr1ilD4kyPQV50IQIhALoGYNJeXjZHS%2FweaaJRWTjO9cy%2Bn2YyYcAML18oNqKsKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FtGLW1R06ZbnD%2BYcq3ANr5ABLfWNgLtekHVBCgKMmFKwMHA4pD2kEggwdMm6OQbJ%2FgwBez2b6I54jzA11tu3%2BlRuDpVfCxeK2ExZPX%2BMomBKuPjp6tEi1vrdfa1pIcxsx0zKROW30YVck3hI3il7vpbrx%2FElnMCQhDc%2FtXuhK%2BIZavLW3%2FU6q48wB7dARjKbo9GbycJSAK8eX9aF6djWV0Gi31dbWtP7%2F6TszhQNS%2Fi5WfC6In5%2B5h5gOGVNbkOZIl8xzUCZUtd7ArGOElrHq8gofxBHPd%2FmV5gt%2FQkwCMhfDHrli6I9DeLsZgvUOMVzx4W%2FY1McCpqlUIWBuhAM4EQ1lCeM6qixiErp7cbBqYsjea6DwsmeTIgJFdKoSj6FvRhrSuWeWw4S5q0unqyp%2FLvMb0OXZp9UKi2YOEpsyISX2LPl%2FjFyvnhLY5xp%2FqNncKk1Sk66VVnQEIGo2Fe7bePg1fibpW%2BUNqE4AryZwOhi6hzwS87ZUBeB7lWKvZaZz2gfybtAZ%2F%2Bv51RmEUZBg1iFMPYFg6kqjpxLf2w1Ko8vbJ4g3rgZePcCkecZXbxxiqzgiaeYXlAGyiqlEZ5KPsUgdw%2Fr1msEB%2FJH8pUrcJ62hF%2FldcQEA6f9nksrR2uqRYtzefGKYy2XB0jDT4t7GBjqkAZ841ma4rGE0c%2BrGXxUsqgI0wUQ%2FUDrcXtT57JnHM1rSOTVX77tPLmaBQjKAm50VfuKSJH9ACsBnK%2F3nkgYDGJjHGFtYFdnsQ0wIcLwiSLygi1SEvzPivUrtdhKUCsDXC48TogwiAc0%2FF6VWOrAmHDqmGz134xyi7O0zTPO6Lh%2FGil1TPOV%2BDH%2FkgzRFIOMLe7Cti%2Brwxl8%2FHZZGJR5nM9eYWLb4&X-Amz-Signature=bd7f7b0d847278fb39ce84eefbaf2100dcbd8416347757f1ad4bf6b65a8c8d34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

