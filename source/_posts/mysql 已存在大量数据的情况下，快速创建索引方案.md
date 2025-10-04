---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNZWFQTG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmEaGOq2Adg0ZUjovQopSGlcUNmxQuM2nwP5MdYXoIfwIgA5xR6SbzO40bLRvqmsa0XQ8PaZmWrSeXJMtJzU3tKv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDDTGVzAOL%2B913B9bOyrcAzvXMC9EF%2BwxaqCbJiWANyQ6CPqJ9LMc0x5n4zb9uEMSnC3ip21gnwkkRjG96SEY%2BfLTqu3cWk1Kp4080Vpxnwr2Wn9Nc%2FcIbwTe2e37s6lLTRvg7IjsSXTTzrTVyZOGwmgUDU8NDvzPqvYZ7mcAB996AStqDknkp11q0UE%2FG%2FukJxXEsCaDtqdyxI16GkSPW9TsAMWWH4bcqAEbBf0s%2FmfGueBOkQKiKYAJ6rQrsld5ey8baKhx3x9Zzi%2BivX%2FE8nebLhYgk7yYiCnJXGdO6zu%2BEMf%2FnJaJ%2FQREXPOYCoLq70UO2eeZT5J0LF%2BEuXZpu64zOo36oWUZZmbBULNi1hNmPZSTOVLlSXHUfDxfIOKe7NIVknzdn574fJOj4CGWewvrBrIdE%2FgEKZbwgXVFEbyx3KDT2HwdL5S0pLLNAsWUIbapKmUS1IHtnkvn6jybFZwHOYgRhEkbYaATIcC0zEYUDEq99uf1et1JsZzaY7%2FVaZLUx5nqdNrStfcywrsG2%2FKh%2F3Fu8xYUKt81DWQn2auBsAE%2FZWHuE5pRlVYmaLX3bLkPJSzjnc2DVLUU8QSe%2B55sMc5y%2F8TW4f5Za3K8HG%2BBZQ8CxvxcR0s13IFjILok78OT0%2F6SiSSdNZLYMIf8gscGOqUBoTHSZkyPfvVAVWTJbzWnQcf%2BgzF9EZFuJS0UH%2FnnDVtw3czTx%2Bu%2B%2B%2FF22wuMWAc99DQIld68pUi46TeRrt8MURUlP8q4JnfCd1fSRtKzcjnaojM4e5mPZ9PBvVebL2ifKL8CzkOy3CtyQpDxaDKySjLcn6yZATAI98FoEhDCWcApgf%2FHGQaM9QAozjaxrn0sWkeWm4LfinM5NSgIecsDxAzRuKh%2F&X-Amz-Signature=fce837481f0458a3afbb4fb6c38e6a380146b5896d51950cf796e880faf1f923&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

