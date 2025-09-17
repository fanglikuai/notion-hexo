---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZH7EXCCD%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQDE281A611EWtTkfJKPLKk4GYznwoSw9811mXdLckd9RQIgTP5G9pqUf1ujSNGun5kjAuLYIFRwdqKTAcMEwxcpYu0qiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBjvfAhfD%2B3udsJwnSrcA36HIO8g6gc6lmSkH7zcIgKMtHFR1w5TsfFsk80sYxn1hZ%2F4R4Qzns62%2F3ZmHZBVHiOSbZEmDX4xfYgxYBZOyo3AxRpf8TnVs4R3FdzqD982hVLG2Bkup2eHzq8zCidKM9yKuk2hHwZkf9OqG6Jn5%2BkKlz4QiFo8sCFQXpzT952%2FTImblH0E7M7wToaVJlhbsIvzIBClbhg9zW1BnvLSWt8CZ8hg%2BHSFeVCy2DTs40H41nRO5GSfgQ%2BAZfV6kwvq2ESij3fasscrXL1JWFsSeAFrlkcAmz4EN%2F4kmjfNxIbB2W%2FCu%2FEuisfQ%2FFNgcerVMBEGMeLN3StGehgi%2FJFsBQmbBqm4PuRDRqq5L9Qx2571LnNCRqkC7NYxlYz2Hb0Wg56tZJLE99TsSdrWYtqfOm05gV8pelOEP8QXkH7p%2FV5mYf1F6pxsbzG6NPRgsyF82HgkZkWgSPwGMJv06KrdQbVdH9sAL%2BYzKNQyfufbJ1e3liAeV%2BnKQvMrgSoDszDKCZ1f3uDCYZB7F8NStyzO48UqfiC%2BgBM4j3%2FqrbN6vS9%2BQZSJ%2FD%2FdjVw6jIVbqcCXy6ZbRVPsAlnargjjtANsDQtEiGSQLxU5ZcOoP%2Fa9R%2FvyJjdgi0KLXkcqxVX2MJTFqcYGOqUB0KXZUowW8LHfgKqKuqhwPh803MLOCZ0VP5qs8igb69jzBVv9%2ByiGrTKsva7NZJ0IiILm3r7C22bY%2B1INvuFFvFIdNBGBHwZp2yIyiB4rw%2BmsBqpMJVJHiTWuphpDrLOPKTtlyLL6szMcu97yjbn5w5Ww3nmT55SYrhXPKQ2Sk8nSKVaynWZSopisrP1AAhc%2B38cJRvYBKHzOElPGjXBS4K9QfeVS&X-Amz-Signature=afee52d9ed1bd9b76217ef2efac94ed6a11cf8eab39e270ff64974d082dc3057&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

