---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5THXP7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJIMEYCIQCwjLCSFnqqouDLskfWez%2B17SrFfj%2FYjjFyDzycVgaE4QIhAKtVPw47FxB5XXVLlQo1OqhgBJ0wul11M906jqUhGeE7KogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxvitD0760er2noqlAq3APlxiaBF567IWiZEvPg3XgaFPLgrX6lGN6XADl%2B3174KPKbmM%2F9uNmGv0diEi4F%2F%2FLKGUeET%2BAy28%2FPG6%2FQEW0DOpCuBaKlTFcAqC32NlNp4ZzKik5EItDps%2FR1n2w7ROCo3QxsSAMGLs%2BzDm0F%2B6rcnQg4ql6YgWAMGpgMzBGBhnbyl%2BMBH5r4JKkk8D3%2FcZ8a5%2FEvJ1CMJ%2BaER82F79kJnHCnFwt8yXa%2FB%2FoLULhdNBwv7MVyeG3CWdGFevPtjxhfRnpZSAJKEaNJYIkUFNCZyqIjsbTzkY7VbX18qeWuI%2BbSi9gpbW%2FxtFrM60P8ewkzwHtxaYW3zu%2Fg%2ByVtdpA%2FbQ5eR2CTfaKx833%2F175Vnf%2B4fb1Ps%2FFUHTdvciHq27O4y5NYYI58mxWDpJDPjsTiH7S6LkhGT%2F2rmyDnOqW5JjGUAjSzq%2FzkPM%2FNn%2FDEbQvF3S4eOs5ol2wEBdUu4s7QclNgAg3RnCtZTGJFai12SS3Q7f21yntwcGgv94slp5W%2BT4WtRqbl%2BEs63nj7Zz98eAjIGxWSj2evfs72x9DFVMcuf057jZmkxa4LnZV5dPPZE%2BQ4xpjEph5VlL7gi%2Faj9YItNwz7SCGe%2BBPKdYv1FhHIP89xEOdKf7eQjzCooKfGBjqkAZ2AnL3VxmomO3rYGxqUrBoooYDIYmzEJWE5mZ%2F8Zlw7gIutI8SBfnOPLtbYWTq78ln%2BKJLR9Gm9zi8Ir3iw3FBZB%2BCI6nUOgecUsnenHXHiQ%2BCYLXTsY996a%2FHWPXV1hxbQKML7wU67O6BnL92ILN2Kxq3yHvhOm4Gd37UfvgQkWELFuT8sjNWE2KoRhq0KyEiNqUfeMIm%2BIxdAFS9yGJ%2BtyiYW&X-Amz-Signature=0a8e1207eb4bff4ec9df92b3aa13e8b220e54bc3327748231ecc5a70300cb1f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

