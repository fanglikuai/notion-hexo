---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665XESB4BK%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIEuBLAft5nAgL5xidRkZPHyahdoxx2AEsdFcmiTqvNuLAiA2%2B6S%2Fo6xMQAi5NjEjCFodrkbilGDugRxEljrvA79DMyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMX2GraFFr8QdDTDBfKtwDdUNTxNYi6bHY4VFBMlz0ZGPcc4mY5hE2dqLFRtD36sO6cJW9e50CnLLHd7uDvXOHN9pLQuOgmdf2JpOQ6GAgYPigBb6O0DI%2BILiq1ZFljjrUO7ZaQJdvbPHpPx9xWT3b0AgTV6MVVVXQtNai43HYIVVxWTkykP%2B%2BR%2BdZJnHBuNayYg70Tc5tztT7%2FYKBuDlfmvUxE1zXFYN0Pd2dl2DmM8NoaHYVrMNw22ZF0EoP8bdsnc8DiTCiUwzJKrwCfbmtEQB21H49qlqsi5iuVhMvBF1oIGEr%2BN5zzrtFi09HsKSkAiiz97tHVTqJirLc9%2BtZk4h29cVAIhJyvJ8cWEphAVmwzbHStQ%2FphR6Kz2Rgd2%2FeEak5pFXQc%2FzSa4JsWxEaBL%2FX%2Blm0WHb93T%2FuhWJYDXk9QeKbZ1kvhSbNdsysOwWmW86oCK24Sa8nfgo2Zono%2FT4X7c1cJ9jiUQOKq6Au37eA1mC1Hlz4m2oblKWZVxytyk4NdBvE%2BcfX5Gahkd2YaxKgoTUZPniu1V%2FsMzZ%2B%2F3oZ7yaDh2HuLHHIkPtEzKtNrxOoHcWE8JQrL%2B2Rq%2FNpltaurP4e5gJ7KsQPAmWfbtGAZX3HzJbCaT%2B%2FHMVzmSFw6zSjrSOlK%2Fe%2FUA4w2OjgxwY6pgEjBCE3NR45kKOf8ZqkredyEOy5MxK6lvC9f9%2BG%2BJ8yn4LbNRJSjCiQq4Bv29pn7qf7xIJWmlRgBzO6C2FY%2BiNOBBMJka6tY924jbp7uRTWLPPF3Td5rectkVsjkFLz%2BkY4ICZgEWNhYXImZ9XkocSk%2FWx9oPIDcQhP49BUj9qZiiR98KB%2FbFPjxQt6bEvf2AFZDBzJyYx42fBYCqZIerY9acXx1sqg&X-Amz-Signature=c672ed3794e465939ee478be05fa00199ac7331720e1398ae9053524a9ed2aeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

