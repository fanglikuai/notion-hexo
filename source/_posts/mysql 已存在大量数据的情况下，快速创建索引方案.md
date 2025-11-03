---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J62YWC7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCQ9F2uKfXvOW66LseauUTAAMgeMRyDjqFqLBpCbuhCuwIgQgIYSDho3J%2F5vpifR%2FaNAOselKxV13q%2BdhUveSkonl8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDCn9foejI6qidGwglCrcA39C3blyM7dUroYIHb%2B%2F0D1beU0RJNXWMMiIcj47%2FPSWtJgHZgNv5%2FZM9GS08IfwdVWEJx7mPh5nvPmZFdzHBBxB98RE0T%2BiHG5lTy86tSm8aCNRSefiosqEFO%2BGxaZ8rSRSQq8Ktrenbxk50OB40fbM7XA1vTfJv9p%2BPskdwdE%2BTNsK29UuiU7wCf563erNL2QARQr86siwaR9QsFK%2FjHqGajjRjrCMqni7TOc8ovqhmYNXTEcWHo6gj8Nt0D5eTSKqHuKt4J471WoOEKDljr4YMrov57i5ZvllDCVQ2W8WbStXjp4d6p0WhbianOvSXOrgpcltmL9E13YSHA0689YIwTdsRHG3ZXDOB9LOO7BOP4gmbKcHHlHNq%2BkMJIDP7NxbE5lqdG5YM%2FAbACrFU6J07clVii6orVyjJAgz54FQvJiXUxRKefxZxrYCEf%2FmKMAq9vkerW0YXDJfe6yzreeqrMsa1PAxsukYTi3Os4ot4u74ZsS3jLgmmLsjYkzSNt%2BzQt6al2ndqsnXS6c%2FKb8845gLvSZY9o3dtFc9YlqZCazE8gZ%2FlkDvSbAP4p2J4M2KwM3L9qm1GV%2Fkq0bPSeWiTlMnkSMAwXuWqGqs0S6H6B0eViGeKGGHViEVMNfyocgGOqUB%2BE%2By8sNQ1UloudjlQNxMsn%2B9PvnJXRq4Dh%2BmtTk5AJSrOuWvc4GlMgKBpqGfCgaQS77UJniTD9n%2BNS24BqJ83wMUD2ut08CUkKEFPJLq4XfwERDvkD98zhn6bWYe%2BTgwOpBRpJQiV9er2xfkhnOHZRYqEOSgGNL1ucPyDuqE16%2FfaBB352qivyiXXRusYhL5a0NfpsoqME18YIs%2FcwIOWiuvSrLf&X-Amz-Signature=be09b9bccee356c900e650e0dc98f69050c1d1a4545b225634a43de6c4a5af3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

