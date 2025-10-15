---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646ZWLPAY%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE3DwxT4UbApUYJiEatmbp8%2FOgr%2Fad%2FIdtrie%2F9pYocyAiBJr%2FxA%2FCcGZkBVNzHI4KDIyrjRC4ep9SopHpYKEFBI4ir%2FAwh%2BEAAaDDYzNzQyMzE4MzgwNSIMp1yDxyukphkluEbTKtwDyJMbD%2BDEiGeTZWLXkaM2B91RmTQ8qOiMN660m8sZJMj2%2FgPQjvUS1Q9gthwujRWMTZ%2FOvqzPK9nrks8L2vz5TupvVZABXHrIdgrgr5oeVupUkyXceWoGmzyvHWrkgDQlLgU7GKXS%2BGB3nqA%2B%2Fo6899OR7J3e6DUsGRgyB2X9AQ%2BDZVZGmcdSKl4inVc%2B5ufgJ1%2Bmn7xvW2p0of8CUTyyZLUjXGGZsEZ2wVzHi%2Frc0i09wYL18cGjEwK8TNcAKNZGmwz1bpKeOZ0bwG2VVmgiHmT6C5j%2F1pHeQ01XpdfeFKFdJwwtQVAwpGfNLHKkyTZ55irv9nCGN4Rrmd%2BkBOjHy8DIxEX%2F79F8UiXlEFQAs1zyf5k5Pj1MfQ9BY0FymgkIpJikGMxBNuAVWPevS6XCBqmtUc9DQhBIhNWNaRWaoEU%2FiUVDcrAMCkxUaubOxs13ucBGNA6qtLNvSI81XVvZ6vbW%2BqreQGqlqLVN3ZPLQatqbD93t5PMPPdcmuAoaWrHZFEowF8AcULvjr8ylEAPrREmWNv96nAt%2BF7sOZOG6lgavbWH06zpUCSWmJH9RlR6v8SBb2J%2Fgc%2By1FlGeUta%2BBRLdGz3tb%2Fr1lWunzf5pStdjjp4YGIKwEOP6Oow9o%2FAxwY6pgHtpMIarkmOHX3OtCq3SydMgmqXVAmdfgwMLqqgjQKlu0BNztEEYTizZjp4MkiOvWej4LeynXNUGiDp%2Bp5VV5dENDFJ%2BjIYPMWG4eYb0vXcut%2BkEaHUjSvjmYRL5FM9AZuFamuSzs72Diy8BeVbin8PmbJUsZcwY7YRc22aV91aTykcmS6Jsq3xc3xASjzWL%2FogRTyShLgsQiLmNB50SFXGjYNgoTjK&X-Amz-Signature=7dcf6405004bbb0b2e8fb41ac9b2aaa64178f006b0a8fa6638e2ca6b62db23df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

