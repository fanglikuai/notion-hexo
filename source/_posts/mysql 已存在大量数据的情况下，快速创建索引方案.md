---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VY25SN43%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBVHtzif%2BhUmdWjqTCav%2B3538vHp%2FY5GXQ2ah3QheubFAiBczjIasbc3%2Bn798%2F8VDOJrwcOohvIJC4kLQCRLBM95Kir%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIM52s2MY4W7QJqgyQxKtwDmDxB2cNd5fTH7jtQpZJTZtHzr2aQgiArdisJ2%2BERJpNhJDJZa5205PCCAawzqUnvBNT4NRj3VDdstuOo199ANtBiuMSOYWde5W%2BWkw5g%2FOv%2BKqGxAF%2B7ku7IAbzzcLYrEut6TUaIaLoyBzIP4D5d8j8laAD2Kgp23qWNjVxNGj2ufmfr5Xy6pasNx6lWgiq2Hnh6tZ%2BJktmYsfQDkvhS7wzYYYu%2B2r7qyDOYjzvSpraM1ESDL8tN%2BG61iA8j8a1gBU3iF1R%2FOnaqyBVprrt9XOJs74JA53HnYCm3WynmAX8YhjQL2vefsNXpG2xlSfVHy7Fp%2BR%2BfTbvfgm4MMQQfjYho4ZMoJK%2FvtT%2Buh8en1rTQxztXL8wh86lBDUN0t7j0ERpIInmANPsUMvf4fRfxH%2BvrIAD3TWCoNMv5AuOfcMkW5aUHZ8d%2FiM8e5v1bvJl63XpjHXe6DIG9gw%2BmKYEiZj5Z8HzYpTaUYiz3DxzklzU%2B0q7HPglnn6KKJ6WlqsY3xSAiTldF9sui2u6LnBv2w2hx42OK6N%2FX0gQd5MxJCkM6%2F7ZfsvQZJvBK6d9ifmnONA621f%2BEOh641uAHx81CLpiijD7V6yzhC%2F8N23YNvxKLTdsgwL8it9EZ91Iw5Im8xwY6pgFtaO2LNl0aBtle7XKBirG%2Fo6N5hM9P40zjiQalVCzncZCalYRLBEGOFKdEBaoGgp3PnqUEnOsxBtBcOYtH1ls2ky7Qnn%2BB%2F94KVxq6HcdS8fpknf%2BXcpf9huJjr3GohUmnZzC73VpUDVdg2Q5IxPXeEOWR%2BycGxsX6993oN02yFBqW0mUfLCotF6S3N5KZyRQ9Eef0fCSJ9fTL888mxWnSbmi4b8Xk&X-Amz-Signature=437f3d02ebd2b694429cb0a4b33190946b0f7a405cdecc0f9c09821b3c5c3577&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

