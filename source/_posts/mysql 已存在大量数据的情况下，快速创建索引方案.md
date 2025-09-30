---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLSKNI7%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQD2fL%2Fk6WlWlWvqc%2B6b4p377RaafZVkoTJaYOtL0T1eNgIgGBSyPQoJa%2BriCGcz7xRzs1Es0zTPqsiBSgPc5ddzX04qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMqw%2BoQ%2FutcLPOkUyrcA8L3EujGp9QfnNbCmKe7ME4JUh0u4eDg8Z9hAP7wQ%2FOiK99wqj2R8SYsqDCuK%2FGlSwB126Y47HOrdYA7RgvMswPxiWNWjKPiqeXBC9HH6bnv9YwkFrLaFi%2BbQDGTfA6G%2BORa9CUvq5ch%2FbYDyncoY%2FmrkgRHfkLtUs7ntHiTeHw3eG1vopiVAOruzwswpan4o253aLEjWKcI53RUX0hKU%2FLiBQCCmaOGuWTX9LONAT0S16jJQvksTGq0Lm2KzJuwrLDq0gKUojPYklMjUvzt6s3Cr4kzauSgUzhDs5fJv6hUew1FAe20R1dJCzvqLjZgjc0k08Wo0uE1RYLrV94ZdI8Sw82%2Fn1rhM4BEkS1aD7hmm4xpVxYMIBaVb6zpEwhQJxfUpySve%2F9wgJ7%2F6cVcOMv7PLl4H3A1U%2BEGBhJZuFmVe%2BVHl18d%2FFKiSXAHsBWKKw8DM2u60KjIgp4K70RGbkWHqrI9%2FoMnS7xcJEuPZu9o3av%2F3ftgLkwn0XyJDG%2Bk%2BlYzJajS4QdnM6i6oTg0WvA3OKh%2BQTQzhB6lLmw77ZOL5vG5HvQPkpd6%2FA8VDDPYUbt7fLZUGTiUJJfEZSSa9vP3yEQik29abCNxeFdFaNNVcqmLMTsIIOahjcWVMJeX8MYGOqUBBLNu1peD%2FdL5xdHKLUz8rMC3vJ5K3n%2Fp%2Byps7j0Lv16y9X7aUGuKxYix81yzWNHcPhqZ37PksWlIGOTIOhZDEcrUqJ9CLbj0pohSycmM0pX6aFiGbjvol6KB6h%2FL6OQL45oQApLhBDpOktqkbGIEXVi6Zu6EIbEwjXOepxjZ2468aH4FgpOBAFIhvLFLKvFxYGLq4NrB5z1wnVWd%2BvaBMAFKZ41I&X-Amz-Signature=424b66796a78293b690f9ed8479f97f44f6dbf5da76bc9819ee071849786e572&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

