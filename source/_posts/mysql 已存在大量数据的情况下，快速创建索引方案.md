---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JBWM7YR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCPi%2FAlz25wBOmqyBl79RubGb%2FouSZ3xq43eS5bNtvPKgIgA%2BdXgB90dQmNTgBDpODjZ4jV0izaU4VEcoMkbJx1m9Mq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDHPag4LjLxE2XthT9yrcA02Z6SoEplcbFzrdhYRuAKqn6MoqkO1t1QGWNEjj97DUtJhUMhQ4OU%2Bi8whQ8k65PyP%2FvalPLUpYXnbDf5ndNup3FG57i%2FkOwUxHhVow9bQVep7uA6%2FwBdlk047AF4hgQ5GuZ4iZ7JMu74W8Co1wxLBemwYFTqanc1ABtAuLfYgM0E8KTM5Ujc7U30RfQQMYiU7GYwk8zOkA%2BvLDHGTgVH5t7rp60HK1qUTu9ZT%2F0WEtFf3ijR1Ra8VlJws8fevr4GurUNhO%2BKYxp5cZsxrjhF6s%2BB0DMXaI8YYTB34JzIy4J0Wi2ghfw0RGwVBIAYeW0JDML8gFY43%2Fb9x2RCL5%2Bc3iP6bVg0c7AiiWtxC51E9I64c%2FHcyfN%2FAF%2BUvuVjsgZg1kjtU%2F5jp1ChbKvKG151ngmctGxpdKt9fBpOzcFsSkW83Ybppxyqt44vlKhvtXDM0vyn2Aleb5UhqBecnNmjcq1s5FUpy4mdOA%2Fck6YzwOFX4Jww9beRur62%2BmIx92RTq4qa%2Bhxoh%2Fp%2BwyEnnvt1sUJ2geumKBd%2Bibk%2FspOes%2F6A6QImC%2BXbezE0UFFFRKLQu98RZlffWIKUTvtZdMrgrEjm2jGsrcfWAxT0Q2YV8OtZ%2BwibQ0y64gjv9MMMna%2B8YGOqUBwtEs%2BCRWb524SxRWZVIesqqut0evZROAfxSlZH%2Bkfugw9763A1N43AKCgPGgKakZ10L2cKWGocZTPaejbQQ2xVoWGKUd2mc3Ynspd7n1Rk048JDUkWmyyhQBHsLSSx47GrlGwdWRFT2yF8AIVbwPJsfwp%2F4zGxwSwlOSy6lSHQIxt7cc3Knm%2BSjxzjyGE0thXU%2BtOK4%2BeXOewUl7YgN9IUhVr5AN&X-Amz-Signature=3b60adabe3ac5e76b4e368bdb6ecdbe84060cd09bc0fb64c89071362b68131aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

