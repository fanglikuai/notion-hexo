---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SSUHLRY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2AMWoPR%2Fx3r8T5ZDY8qTPnq0I7yZxtID2vpbOwCwWbQIhAIctPpLfVh8m02FtbZeKmvYYXXl35DoU19q0a%2FVTuRi%2BKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz9O5ZqZ9BFw9yVgNMq3AP6muGcLFOQwJ3sP47Xke2xzoA9hCvMITa35hcPzYzjftUYQCoQ1fKoWDmHnOBYaDtAWZeg25p%2Fzz%2B5Tew1c%2F92IW3AsFQlJTY3sZdGGSLhpASIvP6jLjhZJ5od1gwfCBBGLe94mVUIRs%2F18Z5bl4Nhc5YDATOC%2F7QQ5Pbx5TmcWPbk%2Blo5KhEyOi1gL9sLE%2BlwjswPdIozjUmhMQB3VUXBwpbDMa%2B23KP558MYKIYnp20o%2FXUUj3uICqeQdIVPVszfFYdxHz4CNaX%2BAkVABXNjTygvzieTM7QfDhTV2Ngza7%2BCw14Pgiib06Vj2tIgWe5OOQaKaRM3Km32JizPuo2phBDz1DO0emM7VWVMYui6CyxOSwznhhW7nqGPefgetnWW8a3ISmlcr%2BqeO5p0NqiAAJO8ao4SAvVXPC1JCg6%2BmDb2nGv4o0N59QeO2sL7KiqB37PswE08mmmKkfzgbl%2BPN7VQKQtjALD%2BK1%2F83YEf7YC26u3pBQRmtJhKOhEs7GLc4BHWuj6UKFizZB72blSf3JlomiQkWSRSc670%2BsQnDPcFxLlSpLC7R5Kk6kb2A5ZpawKbPemTPALoV4qv50DuQaeqsuffDQ%2Bive%2BaixTxE8FqR2SwcPtX3MAnOTD%2BqJ3JBjqkActYvwaWKcwCVYzl1AJFBs8S1gmCOjCyHyCB5BjAKR6EIH8n4gUu1GjZHmblhRoLdFW98IQUDm1x2PgmKqfMIY1OdmIuGaF6d4zNklBvgVPhpB3cr4wVkSg56hB55OABWwiTipcCsqf06jP8wdYDuGB7jcIgXAurzzs4GxWfGoMcQ4UomxZYRpM7xAgph2C9VotcdyuzedXKpVk%2BpZp5D5iuIZzs&X-Amz-Signature=7cd0421d73b9971512e18eb2486eb1695282b2fe9092296cd40f4772132bef27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

