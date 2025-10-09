---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7R3A6QW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T170105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHjlkqMi7xefwZ6yvzuRh2oBvnpyP%2Bdj3YVbsQp7QFVAIgXW2Vd93eZdx%2B%2B5SjlJOpapndpAzc%2FGEztxIszFb6ssUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkmGqOm7aVt%2BQFASSrcA2odl6D%2FMxxfYLDwZBTvHs%2F7PZRnfeoOKhDCgT8656vygDSP8qII2%2F%2FOMIqxO8Kfcn3gvxm6T5hfyziEpY12ZRGKeScm6pQ6ttsndzOIWI5TKVGFjcnP11HUFFe%2BVlLMjRGwdCamh%2FvOS0gpe3lB87HHYq4xrFazAGFZJbwCgP8uEMhjESYsFuI%2BYU852MPs2tAQqoONxDG4Yod41X%2FkIhfzxmVeHOhQFU7n7y6KVHBf%2FM2Vzw4aFv7O6P6NUYoFZ57spEeUlo81nips42zGogfHU8gPquf3mQALeXaUkWnZwU4kVkgM7d3MfPqU9tgqPlU%2Bwao2x4ajmSDXtfcdNv9dRoh2IJSbvnKZSjkjphCspX2mjl%2BgTt07Wug15rIwTwRdThe20iDggIpt0%2Bu3eT%2FzLac1aNEnRbHBWvWHbFiHq6ioO6x3%2F87UpAd5uiFjHb%2FWcczpGIJ8uNtMTcVKyw8Keb%2Fs3nHX1BagAuA8L8sViL3ulp10BCgTDVWbidAiWnMwWr%2BziemeHeqaUrOlgaWcAUaU2ECzg9qdJqhl3dXdvivX7PuWTn%2FgXvANwA8I3b5QeY1pQQinK1GGOPflt6j%2BqLSraHF5MGCzMWkNHWuresBjQHL5%2BK%2B9rLFxMJfAn8cGOqUBJhaknO0I610B9jMv7IeUzN1lntUB2qHD39OXdvwlQBb1EdmvMikUd0CTUgfLAdvxPdcXda64jDyne%2FBArAiC0tDFAn5nx3fmSRvCDGYhWQnNcZyFyOMhBbJaNFUe67DAhJVBlQMtTwGLqaRiLeT5mrXaTo0vtg4CXfQeMGKxzxrzAxptF2npj9z%2FnhJiD3T30%2BYup4wmGk4dX%2Fc0csQoQqmYFopN&X-Amz-Signature=fcb6668a2c5b3b0918df67924857ef2ce80dd8b17be8f77eeefb37886301cd91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

