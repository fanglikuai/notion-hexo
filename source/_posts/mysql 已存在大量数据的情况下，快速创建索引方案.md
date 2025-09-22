---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZAWLBY%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF5x3kl9MR0FYFuB%2BcedfeS8ZrbZZng6TFh%2BDnE%2BXtjGAiA7AkPg1CCw4YxVNo1gYHuhPU%2BJDxFdUteIBuNBsX26Tir%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMmxx3cyrdlePGlOZJKtwDuHI5459Dd%2FAx9ReITF3mtzqMFPwpf%2Bj92wPeHgJbzPzeS4ncDdZQWUikra6ylaJuylXOkGQ4gSuhuHnZosOPJ90i2fzpBfnh5%2Bh3lAwc6uBqH0y0ENFmZLtps1VvHL4OCx1w0QkIsolIhlatbtErt2wi0WqXnQCMuLpgec1iIW5uUOHaghP0T2tzibArE6%2BYGyICyYeg3BtGf6qiSZicBVSsKcYJsrXU%2BgB7fyN5LAQdar5wmRmHb5saNtE%2FZqcJ7VHDGpS2mX4FYmM3%2F8o0w3Gr9RBQ6Q1ODwXukGPZDDFV89tA2M1ZP1gJ8bLFMa%2BxgVJzeLckg0A%2Fcz2FXHEm3DDPvRyEIUrC4D1ZsmPb7ZkOLxFVPhZ73xalYXbJPoeCeVZS8kSWXGUKe0Y4bEF8TU%2FJnJUdwPGw1ZQcFQWDZLRAnhb0AJx4EQwa6XR2AIqyd8oDhtlkQzMkTsiR%2B7R0NlTrBWK6dHysjZiAFI4JlzJ9h6BYTqi6xpGiZcjUyG80OQDfP9vS%2ByrtDIrnE3q0wCahylEwj8izavRhp%2FRVnsaFQJqR5xYhz3rHnOA7TNm%2BIwBfbbhuTtODEuUEeojIh1y6x3X9%2BYBabf0fK1csWBKK5PS4fy2S%2Bmbm8%2BYwqJ7ExgY6pgFDqYDdS28UHv%2BY9%2FShJWjKplHoSxvwAZT%2BfPUuwSRdPErPJPvWNg1rxCjcW5m2glf8carIun6kDGEaYraValpMBBfl3ddbPZMOeYNsDoSxZkq5UzgrQju6mNHVSBpv9umIfV7RR%2B48NS5UBlzd89al6YbQIscWFD5D0wWQnUBqiCuZZXBVKNiaoNVWkRUDM1AXNDU6iVdz8aEqojMLXx7ODZnryScr&X-Amz-Signature=ed7f54539b772951d9b7ab1de6984ca1fdc0f730861a47cc6fc7f5542a25c92f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

