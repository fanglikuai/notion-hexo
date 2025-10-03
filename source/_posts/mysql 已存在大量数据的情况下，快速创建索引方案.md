---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYC4B6GI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFlC0KXWScsf2Ze67Eph7tro%2BQc9v9A16eML2xBDolciAiEA3nhT5VCvVNX0SQYgR42bAnQNe6gLeaTLj5vsa1bSfmwq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDBgwCWT%2FZ%2F%2FKzv2GSircAwQm5jaaYWkFtOnahW9kGcxQPBwIoXsC8wnwVn1ISj6bHIKkqXt6QXbsYsGf7YUYM2WlBteO9My%2FjV%2F%2FP38jWLMR7sW4pyJaRM7cvT%2FZdG3hZvx5crOpwelETw9LYorNgCqFuafXUayDNPlF89uOy6%2ByWlh1mCASMxw%2F3uuahS159xSyragckKWd8AHoPnmsudhWUo5880jlW5zCaWoawQyUiz561%2FCX733tPz%2BvT%2BE0xW7EVixoYoKlVDZCFSyubok74YyhjvM6M8ZNl9Fz6hs%2FAP7BNiUprNBWir1bEg7%2BVyoJCKb%2BWx9PItctdUp4aaq%2FX%2FW3syq7uXpq0ME5pPfnCScB4rD3%2Fn3lIvayjtkbB6GZiciG08LzStnx4GM3xKcXhZj62WTKmG8EFpk5FQoS8nGNYBqgW5s7Zwb6YR6lkDVysiUL7RH33h7tOIH%2BEnjwFq4GsMG0BYy7rNWe%2BncDAFZp2behldogc0TeQpvu1%2B6Mk3imFJIqVNMVwa%2FUJpEKuPX6ubjT0I%2FgAaISkMuwxy%2BWwe1GF8kE6jhdZYaNZTxFVNIY%2FFcLPBScGoFLADrGd9jSn1h9PkxgOD45MbWykVVeW3jb5SlO1NC69LBcAnplksJ0GGb7wdt7MP6H%2FsYGOqUB4OdiRArhAKksy1f1meP0ytJ19HzhcCPaM0cc48PchZDsgGSn2%2FLSMR4Pr8aaZX6vArr2pmQ4hq2RbCHkKDK2MCYB1rhSGFCFeuBdJPO8Os3HazvYIUXXr7I8%2BUX1lYX%2FN%2FP0ACvBnQDwfVJ4Skb2%2FdU5Fe2C%2BaTk71nrHhfZN0HBJqTcjeInkUo2jlt1DEeyw1L8yEYfVikxBt0IpCYA6zVdZPhB&X-Amz-Signature=d7b3b0aa909c78bcf6ced093583a040993c652ce0681ed9e49ba410d7d82e05c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

