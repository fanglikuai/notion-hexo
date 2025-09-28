---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDJ3MEJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGy7LYCTwggoD0KY7cF0wDd7%2BVgwzCqeVPCL9e8vynEdAiBRsD%2B7WS8P1xzWroY66i0Rqu79qIloH8UCKz8q%2F1I1iCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFhlv6EtaK3%2FDoCKOKtwD%2BxNgO2%2BZxwUm8ai5uvj14jSVT8S8ugelVCbveJj1DjMiX9TNiPzHkBuzai6ILHUxb7Um8JmAJgf6FHCDs2Khd9RW3J%2FFLpP6nndwQHe9CAkeM5OAi7McudXDwPVMQs%2Fz%2B46KSpeHAVU%2BPceX36TRm4Qt9f7mqjH55KeJql99slF2ogrplqEH4XMdJFVQ0u7WXg5y%2FKQMCDAhvpG9GWe2v5cXMgJyqD7SmS6eoje5gRIZNnxmKKTOC%2FkUkOWHx7HXp%2FM6eQebAKFYdP4Ilqg2AyFPGEnaCELUGEHSCvMnQCPcNTj%2Flx3ub1r1GOIArUe6HT%2BjWZ%2Bfw3uvISl5gIE6aJcFmweycPrbNcS10e83%2BVkULkGXzKFUUDTgkQEjIE%2B6RqX4PvgiGAqOJV1WzNkm9OL88iGiwdbvWVU3uAI7bIK6Hc0XbSIkxnCdiTKMljHhXxEEJo5jeWlrIGo5EeTy9exuwld7rEiSoC6UyhafrZY8p5mpK5gR9Vba2DqO4UYPWZyptgkFBgpeq2gYXRYpv7x9nabpyGAJUk42dF8TPNmLtHyWKhlbId%2FL5Imt4uK0EpYPC7geSmbAHzijTzS6e0XNiYZ3eHwV1WnH0M%2Bo0P8G4hRbiGUuDz02X98wwr%2FkxgY6pgFgvsO7B3sbN%2Fn09SgcYSQBZV96H4GMVqTDmGMtmeb6eJQ4MAlYluTf4nBgI0xSDsBPGM8Kdtk%2BwxKaMPfHP0biHTBAEnQKlqnnH28Irys1kHxRDc9HJAYvn9ZrWQnXnK8C4NwenBE0BQ%2FOexj0PdI6tLB1kwzupQZgivGtsmKVIUE5gLKAE8A%2F3WlQO02uS9oZADHZhophp5DABKQdWTHUySwbyONF&X-Amz-Signature=9cc31000a29115c483acef4375ce512482f4fc571a64e7b45722b729204d03a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

