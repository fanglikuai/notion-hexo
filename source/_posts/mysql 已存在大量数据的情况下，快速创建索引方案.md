---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653UZ5TNX%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIDj3PZsKH7GZfOSQVkLmDubV%2BqmofTVjdthmlk1kr9rJAiADx%2FxakKIfnQT9iKqRluEYbuC%2FMYIzGIKr7yQzy1xHcyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlwisFd0BoQ2zh2VQKtwDA0xCmk35EWJghPSos%2F5%2FXRo5rQlwz%2Fsj8XPvow97PiGXyp7HkmszNSTc4mMt68ArMYaTWlrceuCRsA02KIaQ70g89zeS6t6pVK1B2akhmw37MzCpIR9VaA9%2FcF3MRsgPipWPVv1OrOcs2AUIk9p%2FphVJpwOVU3I2wlexsKeIU5bkjSU29uLakiyslFF5Uh6oLrSAwwF6MqS9GjyxcnSSqm45ubJHVO%2B6M3a8v8C4w79gf%2BkyCvr9E%2Fx5mqHQBVtGzPGedpzTtwUBPNPaoPH7NPLbyDsZDZ%2BIOnWwzzT9xbO7TQfy547jmsnjXI2yemlfHWhsOGfbtL3h%2FtYJhROmmbjuYwuSmjvcachg7QAG%2B9zNV%2BXBgYDriewtnPwr68BGyJiYXKOKipvw7wAssFvp2CxyCvqrnTnOddZKKsB4sePyQuc1NaJWQHCJOx5gkOXG%2FiiVsmyKKEEI4POba6c6Dt%2BdUqqYMudSIekLGofDlF26xeYpWma0i8SneOVLFNdIRsHYgbPZnrJBMGgQBghEwayXKijtxfQT44F%2BiSYeV5k5x4bY5z4GqKt0LUvT5U8jdxvncDty8KhX%2FJLMxT%2F7j0i6PEWZslvYszVkZYp0B2VQXiLcPBHkZZ%2Ba6EIw2OfQxwY6pgHg%2Bp%2BO%2B9Y7QaY3d9N3%2FhUDS16SyqalS13Gvxk61tTNPiyDFZSHPghf%2BWLT%2Fr85STMTxRtxp6hr794XQ7Ri8EbUoYMTMP%2FQa1Q4uR%2Bt6INRM3E9Fp%2FHlxbOC5pyLR%2FxZkML5UxRsxyCrhSlXZkJgd2AgAEZCzjKJAvdVg%2FhoB4sLMGmZc4wcz%2FUI3ten7GkqrIX7Ar%2BGiEjdnSTLABBbnYTdnD2HAZ%2F&X-Amz-Signature=5386962b1410e352add32ae68d8489ecf18863b37569a627c0b3a71672ae0694&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

