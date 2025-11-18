---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHLNGGDP%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T060135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCLD5VODVh2TWu1hCW4NYUSoft6SGJ4P9qXf2fTLU75ogIgNyLeYTYS318zEuARaydqJPbWGjrv4XNjYwcNqjNvdCIqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCwiwGsOQgakXBCJFircAzhodBJsK82E6rISN2yZPcENJHaDfjxZ1e6URsKGx%2BHaGmIAdhTet0CuQznJO3kzXS7WmMBhl6UgORa91JL1R6pTEekPktz%2B47%2Fh6hQ7gpkpioEb9Zm%2BBt1hgaORWN9%2Bq7S4QNJBGSlGB%2FnzGVzWXZOl7dZjvbki0nhafRcd7WCH%2FbJfJKQ5i1BCjqMAMFNyoRzBde8PlRtvn4k7moKUbIcH7I8lV6H7PY2SMqz6gC4HfUt4am%2FbBcmfd1yUk2QTAdBnkZYIEJjk2b5gSWV58CWS5Di5j49TIxTALvdB0VWvlrXPJz14h9BZnqJV8Eo4cT9za2cxjWnuwVIr0wAd%2FEcoqgTeAaZHjkAJqkQU8zik2WRXxQxheCGil8Cbmn01WLyqVDUebtAuBv%2BCxo8gxR%2B7ozePRDuydYVrvJKiunH6B1ebmdU0Kqo3JqZ35nUi246zZdHtYI%2F6lmNDC2fG2QrwHUU%2Bv8HPF3XDXo6ksVvlLtkFK9FlGnlVmn1F9cA9DThBaoggQdeGid4tA1KjEtbRICbdE4pmj2fKS1uN3I%2FYU7RnsHSHlUBx8i%2Ff0AGNz914cp06OzXAe3qLJ61mfrtI%2BaHXHl4Wjcrdd0PdZPpoW7Ex6B%2FyrfLZ%2BNG2MIL878gGOqUBLudDmQNhUhZIENVkpasY0w4wLXK0BhYDYl%2Bj6PEd45qhtJyoLMIGfZMehETBCzc%2FDWCM%2FxSo2LTH1se8tXjtGISJ1%2BZd8tolcB80iz5hioPMVKQZP3ncBUFOJKpdLy5hE9RmtjF%2BYKGLT%2BKzEk%2Fu0IMHZ%2FDDHYYFmeroItU29WpbRXJJ1KDhxZpY9B%2BJoPmitBb2iP3osYNjgr%2FU33tTmmuOCP0A&X-Amz-Signature=c637ed346bd38eba1b984e8ce2d1f2653c3cb9bc98440a42a728b93521e38be8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

