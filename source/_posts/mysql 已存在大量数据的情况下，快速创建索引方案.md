---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SI6KBR2N%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIGmgbueBVh3QgH%2BpU4tN70BDdlnCxpJ3JQNekWh0j7YuAiEAio8SZrVxJUrIyWT9LTmfOEsfIVZrI0taFZCRfTIimkUqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPrUIgtpxABY0gfZkSrcA6402w1BClLfo0NTobTBWrlSSdvsMk2lISUwZB8K69llalWvypmTZPfQotJGAto%2FlO%2FqeJqUd8BVFlNGES23umVjjyj4RBC1mTJJEJc7JojG2AHKxdn7IlCvdReQhvQKDQV%2FAud9VlpDH8wH574VfZ5gr2dCClRVSjB1mpPkgTeNkR5oWAR32vkS6Q4ycodVxywhDMINc%2FVgZMKsTedPSXfJjC41bfxlrxy5HLZKhtoLJoTbZ1N6jsoT16dDQ3zRob5D1zYH9nAESRBzwTDrM%2BmMqBbaHon%2FPX7NwSEaO7xbZgY5k%2Buhd0pV9FVlgBOeRYPVrjvt0XkhdcTKDSSWgMbpjgZiP%2BV06uAZqYoj7Z3CNapUfSa193G6oZiPlr6Jfyh4avFeS9kLdfx3%2B2%2B%2FN4jkORl2VC5I37CNA0gVmjQZlXOm%2FJKNFGJyDfKJCb8cLMbo%2BqsTVJ1%2BpWNCJYTi1zaAW5GuDEWgrFN62xNnHVCDia0Xwn5CsGH46aGKy6QN04kLHiXXNyLpCEQH7XVIl8a0c6E560%2FTz1lGrfImombBIN6GseaQD8cqU1%2Brd6eSYvNEyMkEzA%2BW6maJQj8Bpw3g2OUVHuXrqOtdSUCo9DS7%2BkQWqcp%2BENZnDtahMNea4sYGOqUBOQ5Os%2BuSMOpMd9%2Ff5%2BsDKD9ogB0R3I0mkDP0He5oxyikzG0e39zGFVCWvgFQNDH3ZwuRlIWPGnuX4tCX9e%2Bk98aOjwgCQYtJaaCF%2BjRXxT%2Bv54RTBgOF8MH%2F4n%2FY4tUXNe7GGvoSGn%2Bfl05%2Bx3HJgciNUOL6sAanfX7SJ7MvpkE8OufkPGPqHeue%2B86qPUIJ5ykSJJFzKN2KmA2iWbzneG6SztQs&X-Amz-Signature=45dcf77252157b94490a14e96bd5cdfa9b60287955b2c0007ff3e793bb74a328&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

