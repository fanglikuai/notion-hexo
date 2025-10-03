---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSPJ74NI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3mtWe86HNRqBBG%2BIPvJ2fFuIjsfnC6OlakG6RlOvBOwIhAO%2Fu9cUqjpWb4V1HOeHZMn3dDwm9yIojL5SH1e%2FLXa1tKv8DCEsQABoMNjM3NDIzMTgzODA1Igzj7%2BTJAx4gU9HKvh0q3AOguatVliJ8BKdxoFnXM7dTxtPoVFxRqnjtW35elgCRgWHHKTM%2FvHWZxNFvmEmv1mqvbYuAa2xgF%2FHT2EIleMkPbYrZiaUA9L%2F36rbtL6xFIov6w3pwEMcJMWqltTtZB4S8tvfUtAKjRP2mLsOFJ5Ekl2ZRuEDwOSYOJRy%2BoPtjA%2Bg5Wi6BnMhyk5UM9%2BqbS%2FOP6tLPnYIRv9zEObSSmXhnX16IkioyDzVbIPYn8YeSauKZSd7tmyahTIde0CZ2QOuDuO%2Fz4KcYDcQiZKdpdqJvGKbClg4ijL92lo6aRDHuQCxQ%2BPbhEhb%2FYOqfhV%2BjnE4EKkhG8BuOZaOjt8J1ox4a8R4keBR65Z10ZDxRWqPQrWgoVlolEGM1XHJMJOLeLoBiQXjkIzqBo4IAJaBLFPA7umAvYr7HzC9T8b7qkAGXLQ1HmXICApVQXCvLEH8XJFuvUJgmJ733p93MzVbaxzhFFpYDqWKE9WUTObptm4tDD3Afi3AilEmj51X1b1Vvd6EkNl%2B4QvP759c4B0w5GNEKwYUuY0QYizZwOlmUmvo78CFk2YBBGV0%2BIaG7mjdzHiazw2AVlxvR%2FoIPnkH7466FHvB%2Bg%2Bu2pGCX5jBlDnpM214xoWU3yiiwH2iXKDC5l4DHBjqkAWy07QmShhd7z9%2F%2B7aUSqgSj23cBkNGBKXyAPLk105nTz2hzfQpfSelqkjFRzDq5WWd4U0%2F1PfQ3WHFvughHbiZDr3rvbVgZihwawkB33nNcbssjcXimWu2RsUGAqI25Bk0wXPsZkS2uKzHZlRqJFEb%2Fh09Q5NbpaXFcVoA9WyteyZF02vl6LVfxSPz8QEtqjJE%2FADi76xDXyr3tP3q0O2rNOEnW&X-Amz-Signature=83fb62e75ea8628828f9c61f2c52b38d85866390a7373a83177063828afdacc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

