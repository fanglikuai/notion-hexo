---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646BDXTEV%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrFoB%2FmefvkiFK%2BKwlAM%2FHIfmhiitLSFj%2F7eHX97rrHQIhAK2%2BW%2FZuzeQfdREhtMmOEZrUWVJphX4ooS6Mqs3Z%2FDO4Kv8DCFQQABoMNjM3NDIzMTgzODA1Igx%2F2ju2lb8H7Lg0IZ0q3ANB9GuZRfe%2FskRIQuuWdeiVqxWFtrrLXMDfTMnD6odPIh5QscMB%2FruNEcqYuLARmVOIOckCPIY2Ftwr6aG2qCODNNo7%2FDjF%2B2kU8IlcdvlYSh2MkjQhw7Oxv9TUa2%2BWu8RRBw9MAlmMwVcN18Q5XIuU8Y%2FUCCzDeF4eOabkPykxg3wYyu2LYnSuc7QvZWsgIiT%2BSK%2BzlE2LcdYI28RSFQR8lnZFxhqxpq6hc2TScugiy8mE2J6vHLvAhANbDgcRCegb%2FkudT%2FZsTJSGipncxi3AreeoLSE%2Bqu72zdgxBjkl7%2FOp88iLAEzMW256DPICA27rh36pqCC7HOwCB5JxRzJCHyzFLgvnMBAYbxliZJac2ivtxCIlK0A3m92PMMYwc7c7QqjurB0HoPMvV9sz2k362voQfa1mQm76V2BuZLudpw66G%2FbIzB3l%2BNfEPCS%2FUQ%2Fb3IP9BtV5VlsAaDIVcjZCP%2BzvaLDj%2BA095jljva05O8Je8dnGoZWk2yiuep9jP3GRVFoccZOzbUoHrxF4Sl%2F4JgmALBP8FX%2B4lFZQlyRrZgtyPHkqgMoF905GxldmexZMzAK5PQprGvxBnd4YcEFEzIgrgjK2CBUO8oFbIncl0TFxuC7g7CGjrQMGBzDmy%2BvHBjqkATTxL4y6hD%2FK3FCz8BBqKiga1XfLeYZjGC39dJ5khdS%2FqJFsB1%2BJXRX%2BNbFbjABOHH1diQxRDvOOLc01F8cguy5GD8mdxUczz7DYWEVntYMtCdCBXeLZsgMCUTcruxM6ojI9sMyPNB%2BDujMKISuEu59HibI1Am02ueF51uApnj5%2FY%2FZxMGoGmI5j%2FuXiR8wF0%2B195AQ6AulmrSkl4%2FC28Ykus37a&X-Amz-Signature=600e66e60da1445c57fe547de23d53f96c367545f7c547078cd745867ca6d096&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

