---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F7EGK7D%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BVSgG9QFMfxSBrAnsY6IB%2BSBIIsB5QcX62KYgCDvjHAiBkvTb8ESONeccwP25EmpLrArp0gASwohOJrNraWlSUmSr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMcfCOrjUGSYrYOkWBKtwDswkWEbLF5zWFt6YmE68g0ESRPefp9pGApGTd2t%2F5sdSpcplGo%2F9oZpAANBjMP1789yyevJu5eE642Q0IJ4wOInTenArnPLwzUq0c3ayoioceWoFBxbZhCUeqnJ6NXJp6DaQGWGamSjfacHor78BC%2B3kZI7myGkPgkt%2BUMdPrMDk0cx1VnkXV2BXbaSmxmcv%2BJSxnG622%2F5MKsYf8f%2BfK%2BUEJQAmdcrG1IM6KdQf0UYmFj51vk9YuCZcuT2zBDES9yA3WCHDpaKN1snCJQKa2wxrtlW%2BScfTI3JtOFhYpg8EKXd4wXHhGnyrMsW5GZvWi%2FMQdwl5h1%2BFmUwPF1J6XkBo7I9TadVj%2BOIACR%2FpiHuTARGez%2Fg0hApvYQnzZI2vIlZvB4R%2FKtEfNQiJcmW8wBoBS6NfjiPXzwWuadcFCyt92CkjUXOCOWPr6DX0FfDurAcfzsW6R8ykPz%2BDKgTJCqnrD5JFhMyC3v2vdOcHdlYoy%2BRvidgA5glyATRTOCuj%2B9cj7Rq%2F87RQtZPYFLk4AfEo8IRALy8fCrL%2FsIlR3QqB9akXmtcLvhSgUGBqdNbvrgMiU5WaN2e1PkGDhauByVqH06Tt2cR4wxsPEpMkC6Exg7cZZI9zY0zjF1kIwgZPZyAY6pgHPP0gsykxpQTRVPrdAOr6ZxE0qTubTCOrs75yy8FQ7S1Gt5godkpDMTY2XTRpgB%2FKzVV6KsxdI%2F6kvH9oJm4aw25Sh0CWwyhkrruYhoIUWrT4qJQQ1fJjCPyteEO8Nd96Kr1yjmOswkzBN%2BHk2S1X0W%2FnFCnKlzqK7eJkrG93rQlRRCYpMopn7mwLV7R3efKkk6hUJEE0m1FbAOaNuqg3AgdYiAU3J&X-Amz-Signature=22d2932ce368d29d117eceeefa32e2a4b9c75d95a4b53fb0a04e6f092973dfa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

