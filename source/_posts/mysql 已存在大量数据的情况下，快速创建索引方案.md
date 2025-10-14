---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUQX6LS2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvK%2FQJkdkyRnjCmalLU2UXwgnmw4DZvUGxAJ3%2Bxwc44wIgdYOanPIxUppgoue13dPK%2BLn8TKX22KYse0GrhcM%2B91Qq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDNHTfIAK2tORVpHDHircA4laScqPOVFbjgc5oDjYfgngTwQ0%2BeIUMJ%2BdA%2BZUCTunNndDoyzg2FLcDLyDNLUT%2BSvxRS943KP46Fk0uZ2cThKKbF%2FHV7yhTZxAjgxjzGqwx5QgFkuPCEKbc3ZJuYC4edxEeyj%2FJsEQEO3mR%2BeABnDu%2FSldDC5Eg%2F6XeW4sqqFlNF9DK3jCKB4uVsPa4PA5u87J4pVkW3jqJ0StiVxLsA9tH1egj6OAG52MXx%2FpS84B5liKqQVJ7JfYuQ0%2BFBFvp2ysIytjrnW7%2FHMZJRvjdG0wUdRkzdojv34FbMsbByuUsR5xv64Ys3DjG2O3DmdNYNfEaJhr6fF5lffPNc4cS6e%2BRkqfnLuA1EMwCnDnMyNdP%2FfbtFf%2FXPtZToPx7%2BX5en4AKjMlyWUtqOMViv4nIkA3rLXqbqhKH0q5dAAkhBw%2BkjdKPxnloVJgbVmESw9ahtQZ%2BQfFhzoA7Viun3WSpg5O3AMfgHxaeHQFrVVLeTv1y9rWyLm65OgMtAezPpfb7%2BAZ9%2BpFa4vYWggjj65DfSPpZFCrBkxcSI8Pbj9cccHobWMr%2FFrgrRPZmklApyCe1C5xcXb5C8yKnpinD%2BvpfUEVC5BZTys6D%2FdD9NB9wdOKLh9V41QB%2BSYbKCXtMOeAuMcGOqUBzdEdVQAhvqgNbbpN%2Fd4PR0SlN%2FdThEalR6Pe7ieWUnEd%2F3NJhg4rWsFuoBSKI5WTgeWtk5KBVIDPzjKGtmHV%2F5y8oozVdi2U52KoEW1CsIcNjU00ArQbw%2BOdfjWBi4G7dNB9L%2FlBI8NwinQQsXQIhUxmyuJso3o6m8cTUYvZCHjn9rIURJEwGbOZKPMgZ9%2BF7phN4dcAaEfdZALpYwTu8N9U0mFx&X-Amz-Signature=044a36a02cfb58613d02dac436d95c490495c6e03cefd5312398065561d2ddc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

