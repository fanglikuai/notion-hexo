---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI7CQJSV%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZucOJ16aHsaNrkXBU9ljQ7z3oZBH57eJCmkNgUjDp8AiEA9gjvgCfuleCKuNiLml6i5YhEPVludPpnSC0PjUfQO78q%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDGfTWOauEKIKmXNfYircA41H%2FqnzbCxi%2B%2F7nT2trg5a0QkdNOpNNbx80uSfBCW5%2BBVimtlyLjgfl1z1SUrNDUnxIWjSQfoCzXJsePfMAYjNunw%2BApy6Pf9gYMiJCVUTPx%2F%2FeF6guPU053CNGegpLut0z3lJgsQQYNpI8jX40t09cCRvO7Gpc8XIV4oIitaE1chJ8p0moCCgpy7k3PAh1ByiyBk3iC0SprsjrqrIAoPdHxr3JwK1cOuicdICylj11gGnVxrLQ%2FUSM7XpFqOLfk6tm7losrO8BmaOTf%2Bw6SR1w%2BlMr16K%2BEwIy6VjplH%2BqGS3O0kPw4d3y1WEytLD7NUVVqhpcRYwuByyeiG685skT5LYKwMfHPjVnWHRgEVVbbbmYkagz%2Fb7IxxsrAGcW9HmKLL6FQha8Fs%2F6CACMzG5iMrYvwox%2BkEGcjaZ6aToJBRRoRRiFG%2F4y4bfmr8z9xb6onENBpxFa%2B%2BZYoplp3trA6UYoRAaNIQ6gZLHn5C0YNMser7Ubc3f%2BoZWx2cA91r0Le0ahk49DFdhgnnf9b%2FGCq0ghlXAffNwtnZpalxRhsLaduUSUS0pLJJdG9IJ0s8BGDGlA0dveZA58HuCM84ZPIZsEW7rUvy8h%2FMUeeZI88FAAL9Dk%2B7GNFhXVMKzFqcgGOqUBwyqwVwvkoc%2BealerELrS7Oo2OWJhwBanS%2BKBo5in%2BADrc5HX4ZKVKIVpHRCvUpD0C%2Fvae%2BVb2sbCbWVxHVgLSxqG%2B07VQSx119JsORSnqWgsQS%2BVMITaeaRF9kP%2FXPADNbLBjLWrDHQYu2UPBdQAad6JVzSs0NZ37uYlt%2BieyhuG4aUfaEpc8mm%2Fz5%2F6lj0erPv%2BUOZUXQSOVzmpFjQbmvr6nqmw&X-Amz-Signature=368927ab0f3466cb528d340eb72726b751c3999b71daf41ac0e81bd35d224748&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

