---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVKOV62V%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCIHlC9XBvuYVocBvrpTBp3q%2F%2BQ%2Bp%2FKEuCSikG2JtbAW2LAiBaPVvaAFtBSzIaS7D1IgCHNy4AOJpDbsAJat46fIHqxCqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAK9t4cVQBTGh4nf4KtwD%2Bx6dZ3GR%2BnM9HR7scwNYKhY9iJ0RVuy6XpNEUTouimEp%2FrfFT8%2FXSmuuP5LlBtb8xaRgwlkI3p0hJPGz8eeg%2BYMtm4KWeHkSIn9c4thkDZgb0AuxiW8UouSDBIpPinsQ48fBI6ph%2BsgBiyKbqyudnsLwGqgeJzVixIeQ8l3jopQqjZk2Es62DsS%2BuxvkjhzQGqnKAOkEF5aJffzJNC2clnCAWUXBQkhCYWiKpz3rpE1SJIdA9xDOKrHJRfqk1eYeanvBfkenLoaP7NDnEX7GErn%2F98ZbbpgjnsMpfPJjtaxwiU2UnSoo5J6Rv2KLunhPKuq99xzrO%2FcR5N2ZVIOqFlOdsMwkI3KnWpyaskLnF7%2FEoj8OlrxLE3Q1FFT7v7VjLsD%2B0J5SO33%2BX7fFOJwyCKpaB6weS8YznoXoXeIP664zXhj9Av1V821y2FMZKb%2BoVN1JcQlhoRca7wUgMqjfO0fTq5pV5ZbGRed%2FyOlxPYro93mx88heIjQ0YihQtuTASCqTBc%2FrIcrjo2sXb4TUhQP8R21iHWGivRLr4TyeSGfJia3gaBQ2aG2SxQA4Wbi9r6GgzNmsq0uNMLyQCOfGVUlstunkPJiChUi%2F6lBhB6FWZrcSVdlUxAaVUOcwgqn%2ByAY6pgFfBzd79MD8Md%2BbVE%2F8BummXldFDOImjO%2FFp7O47lNyZZcgzxgynOzYyBjg3k8EsCFZshaPT60Y8FP8hNvGgmc5BveXmK6bQ413JVsJ9JWeBG%2F9bfk4DP3%2FAkbBMjS6hHmOGzrwbzaPjLXa3Fx9ZJivZLVL%2BrihkFLwkhPsLYOiUObWfnSlgosTk5V1NgjK6eAAxkJaj1EtBa91IqjRUDM23Ii29m26&X-Amz-Signature=3115e552d4f3e6874f46fb80b8e362f6f66ff61b2266d21c9a2835d2cccd9c06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

