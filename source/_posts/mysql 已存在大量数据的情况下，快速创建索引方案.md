---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QC3RW6X%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIB5f90S9S%2BvDq3WeOj1ZDIQTD02SbWrS7tyfqBXdUj2FAiBENPp8kKc6EVJioAQPanORQQOeV1njrs072uItze4EkiqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZwDpRU1n3UvtE%2BWxKtwDKBWnFIokcZZ63P9jMBP5Nj5ufW0%2FFTD5ckTwT%2BrDWqo4K0L%2FIflVIMxQNm42YsDIK1IKVbX3275%2FhnRxGmF9o3pK2mxEbDUNtmD%2Bz1FY8zdjpxFQ3gPJMD6oiLbm8MhM7ZduZxnctkGJs5qclNHfQVhtiBkPguzy6e%2FXmvgY8fANcnkKDR080QlswHCGNZ6EMxTxsF873gXvUzS4PqQNyzoxDV8gUFDmbKqScczWwCMDhjFShFwWJUJ4gnsUTVpY0xTq6nRfbq4x7UFujJnb9N20Bkf4F7U0Q9IjQkQk0iKEtfMOwhMgziRh0CP%2F2%2FBBa%2BPV7SCGMEQHa0rYaolW67ArvvrBCq5Loa0YdkpVdKtV2oYKV12C8Cm57NpTYfE%2FAcO9cOYNu26zG91%2Fnit7qeWDQClKxxOEvBN5%2FHuhP9fLWsgKZoHQ1j3IY3Hs7P13IDLH%2FLkfMyOGQhni4fWbLa%2B5qTUvKEngBLmxQPnMpcp%2FJk3u5%2B7gBA40od49Pql7M4zmlo5Jxk22c198DZCdUdMM5YSWQZxY7aZ3pOo33v%2B19RFuq0u8b15ehHmncU3hE5T6AjYkoBDi1eKAZQEpQWwhZCAuJCMXvSjHYFHliwdLSLNjOaxQsV9Tyi0w9ZHByAY6pgHTMzqmuYjOLZpsIRa9we5M9%2FSBemMMotD69qnbU0zwpuWcRqbU4v0nmR1cN%2BJIl4bH5kPZDF8C66c9liVjDHpZnTfefKAx8P3aP8LHvGQ%2Fzn8qsUplUwqJj5%2F9YyVmEfAd68e1mbP740K2fl6QW6AMy2rnLfR4pi5VPrDetdOYtwNM4OrQh5%2BNR%2BCFeGQmLA9mAzzIWSrTQ4o1CO0udt8SNwg4RnVS&X-Amz-Signature=83cada9a41405294ad1638786024f83e307cae29f0388e0b6920155125442d6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

