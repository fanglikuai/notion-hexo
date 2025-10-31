---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLQVZC7M%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIGlzKYSK18sxrb8prIOqOeVKbfd0945MtTSdLaciarhwAiEAgtSdI1JPGrc1hRHqmXkeHHM2SecHWVDSo5wzqyJFtQEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDEijubBrhoKyZvuPvCrcAx86DBTGN52H78M4D0e%2F5yOvTXpzmWbWZBbrXEbhTePhWsLMfcGyZc4rV1G67ScnAXCkTaEXHD%2Fv%2B%2BxP1PWBQBKou3Uo6wh37h9OuMthuN%2BIYIo53s07ZNl%2BQkTX06QSyviwlaI36wKTolyrI9TesAqdpvIXSOFO9UJSMeK3fQ2NuyOpq%2B8EAyh2LpvjuWh6mB%2FiazlShk%2ByHUmyO42KzbjAM7cnXHWV%2FvXhHhVwd8YnGeauwsObtq7raY1J0cnDctgGvvENjLT5tDOYWyPJTGatZVXWlCDuf8CW7sC5ZQoVVTTz1iTXlDK7CmToG3aIv1w4v6ndvW3HReGd7roccteo0KodnBh%2B58hPgtuKQP1D%2B0%2BQh9oke6icTeFeh7194jKhTR8KCpOfAgIVw0XXspKnOztvwWp8WuaqjdxaCgB5N4JBh3geEzwx0bAIIwdlY436H6LJFKzoES%2BTxQugGzQh8qRS5O5pRPVfImDxBsi0SM1LnkQFDWhnl7nKVscdr5cqOvKypRoBm%2BqKjFa1sJPWkzKynY6yY3fWIHSrBiPs4cIS7RIMe1Ji%2BQOxv9FrodNu2HqTGi7DwUIF%2BmBUUnuoLucgbd1eI8MEVx2%2B7gggQoDDyqqVcIRSc6d6MNaSksgGOqUBcaWN%2FdAhGQSLAbfgDFeHk13FADnRG3NYQswkgyOZzh%2Bhcy1GwKEF6yHCn0Yx3t9xDNcvIx7tF2aWnoLzgQAZnv0of5it33O80Dxf0njOk1Q0DPSpZ57WrMY6O3lFK%2FZrbrYUGBHVGS2f1SWnC69WqTMybZTUziCb%2B09Qw9RT5rPAjNPEvyUx5typBJzS1jHN277Aq9pU%2BGeFf1LZYqy8ge%2Bgox3b&X-Amz-Signature=b74012c6fcbe9eca203859169132db9b64a2838bd6d30f306ff55d2464077e5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

