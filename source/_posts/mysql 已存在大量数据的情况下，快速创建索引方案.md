---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFGPTL3Q%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDALyi0ZukRv3SRB%2FXyygHge9bzSHC9x25zGrMjKJJlGAiB4Rmk1aQk8yPPjNE3CVdCbh2oSv3QP4H2wxB5AgNL7iyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZySmN5hkGqKUrobvKtwDKtbQ3OsGfDWMJjoMunRwqwNR8LGrcMnAnNroIpQQ%2FMjNV9bXqOk7uk9HA0KOw7B0BVy5MXp4KQq9U0V48UcclWRD%2Bh%2BvHxn3FQUrlFPCy377TNj2mLlgIpFq6EoPiHXPeADX5lt%2BvCpPF3AVzCsyi2xFCKZrrSQQS9lK9VPjYoGoDy2AEi66rN7prQASFhvlZja%2FrjvXIQ%2FrpogFMpBpHS6ChR2AtW1DTmtQWm5KBso6DrWTmQie2R7ncI2zZS%2Bfrv7yj8EmApcM2gE6JiiO0x3eyqTL9sAMctw950gGbS4JP%2FQtReKKnJuIcLGTs%2BJ9ZjESVp88AAu%2FJxKGzO3rmpte9wLhYAWw%2FPdQTIwzQQ6WjFDllOR3aakrr5jVTp0Qa%2F5eeYkIGFT%2B0ce2oLuNCssOW1sSTdBbIkfJCjivMADLd6Wh6YbdbArwHqp7RVAjK9N9gjYbqV1m5MzhhxuLs98aq9t%2BryLVFL4%2BTH3NiD%2FVMLjP6yuJObM3cxC3b5A1BzZqt7S09tiwU5I%2Bb7vsm5I477mBrZliuq6Cfebm9RHuwOzEDqMSv5czDbaqLlZaJwpC1waXalHZB%2FnQ6AYIaRskk3J63%2FYzgf050z3tgeXl5g59aRplepUGmkAw9dqtyAY6pgH9qWLrLffTj%2FsbfkuPNdU6dULq2Y0RaCN1qTLh6J26ktIVgtlSWM4wPp%2FEq4MMb3gYWhwPzPOtm5wYy%2BAIZfqFMPCu5oaqfMVTMQNTk2aSJnlj4eyST6g2dKgZaPfOd11iI6okuekejk5q95btKTpO1CSOOi%2B198jAmIqeuz5cLs4hMyykkvU%2BKFmcSD00K0J%2BZaHUlCQ%2Fiv0MeyediYWKEFuvQXJK&X-Amz-Signature=8f47d9ccaca5504e63df8f66be2206e064885d9d15325ae363473df2e62bf5c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

