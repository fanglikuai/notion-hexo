---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WJMQ2IOH%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD9zVn%2BEGTDkRin2GaUTOn104UxYsWX4I709Kl3dA4FHwIhALrazpRqspWOxQwzDvFfcoxQ4Ug%2BuKG5hgYMVDkfWc%2BiKv8DCHsQABoMNjM3NDIzMTgzODA1IgwZqVlJLCPbiFiVy6Aq3APvoXOb7GIw5OEllY71sokq7apv%2BtOyNzx6w9YJUBnC7CjI%2BdMk4QXcjuJbQHzbDOoyN3l18lIAgG%2BZqcciLejIy22o1bqe%2BCN%2BLc7zZ41GFPQbMSDwDaVKKZU%2FJufpLCgnAyCwi9BTGAVhnC607rmzMjggKp4Iz%2Fci%2FnBUfdL8SwTy%2FxiauYRRjz6T1oaGkgqhBh9hnbMFCccUSfWJoEeL9F9YPljS3wcAWOo2mc0qLRnHRxArVmyIf5HqCZRua7pcSM9TLwrDIjh%2BMkto3Uylc1QcR44oAz8DNOvXi0DwlY6%2BeI2A%2FymNkKfU5Q8QIrm8YZ85z5jGF7Q7d%2BT%2BMJYCM6v8SLu9ShKCTEeAuCmLUSSh7STIGRwe0WJsVWQJJUT9JOlN%2FqGoY3%2FIRm51r96%2FpA5K5euSOQFrohe5rEyawBdb57dig%2Bknj5yrNOXPHhCLZJXAvpb4bq1ZckHOYR7hP6OvBzM78%2FF37z8984zzOvNVh9Cj4ugPCgJgjI2nMfILcOssNM1BQmWmJfhmNu7r%2BxKK%2Bot%2BLlsBtmVAOewsApvSuG%2F67D1fFzasTyubv8d6CUN0Ewgnc7ejzRs1l7LvN5yd8O3O9A%2BWSjz3P51xvR7S6rkAjYWGc%2F1zEjCM6IrHBjqkAfO0VCeGUSbQ9fSJZKZlTMJ6WH3BILAp5fCfN6axASCUg2ofPLrxKBEDwkYqnbIsiDUII8R4MFmpEO5KFk4%2B%2FiVhEtlhAJdheZo1zv10SWNrZ77Bwb3frmFCLo6IMZsbFcFZIJU%2BzQv4x3Jb6e32Q1VxFIXKvfL1eTlQVhilVd4cGJIDWSGSSm8UxTh0lkrkzmmkltw5uD%2FnweaO5T3d9A0IGVzB&X-Amz-Signature=333cfcc0a894b4e9032c432714f6447e30acad6a09ad976edb2663f27ab8e205&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

