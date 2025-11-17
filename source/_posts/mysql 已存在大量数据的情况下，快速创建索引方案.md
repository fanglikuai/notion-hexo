---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXBQP22H%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T030048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIENZM4j5yxwB74zFPmPQCksOziv%2Fcbll4wEEf9n%2Bg9EAAiAZU8c3iWUyLFpFsAKuXphbzvpO2CnPrG7WXyDBFkOtCCqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbAzPkPEqTPoWKbO%2FKtwDjMV20BAuFw6LGA0YNanyUys%2B8mkMMmlXssxQyB%2FvRfdWiJdbm6yxe8U2uvSKLYkiXu34la%2FyfIkqNIkHiQ5QYr2n9ToSmYdvQJu9VKAlrH%2B%2BbBQbpYUrVpEuCl97TZymwXlyrfr5YdpNBzDwmwYngeuGUfN6LPCYFBEES%2Ffa7c3u9btQykJzuRJce%2F13oLbfLEXrwl8WEZN2jfDAbey5kXKnZWYnWyQOMHgTdxzR4H7vJWL5ixCmyLE%2Fyuzvmyunwjz06IO1YeA89vUIt1dB%2F0YMfpVB5yJD8BBxcBZTMvWK%2BDPLtLdUkaivT3%2B%2F5iLJaU2jEc6QQ2ocwg0hjNYqrarh0EIcpW26f%2FFmsx%2F91F5T5BODOzTgvWREPdi8tjhsDFYGQBsqsCO5fq8j9QhYX5U%2Bqw36%2FQlv4Fb%2FpxbJbFPBZlNCIStgiI0%2BQ82%2B%2BX99Lh%2Btfe1DNd3Ip3zyTzCLc%2F4gn0rCX2gAnq0RN64%2BGrfodw%2FCyY4BDlkF7wOhvtTz%2FuEEt%2BV7MtdXOo9bbI4b%2FCTJddbm2O6GybeD7ishkKg6anfEKDw67AgRn4CwjYY13C6QMLC2S7sDfA27UpxGxO04KSDK0DA%2FW9tpHkAkjlWcibwB%2BWfhNlb%2BGccwqpLqyAY6pgFDoK70Z9ulJkbMYi%2F6h%2B%2BEudTo7dr7SXQn1MVYFftQ5xgnDREqCWYLxjKas6ihOrZIzntoYnnl4l8%2FDTh6YDRtae7LUTYpwNpvLcTu%2B%2BYUmmdTC%2FtfIjdX7n1zKwpxvHOU%2FGFKDfKrEnxG%2BuzYuT9S97YU5VFn%2BRlrhRP7GzDL5tsvvovEpGfaoGCCxncs8CZzJVCwRHvO5X0qqmJ699YC7nxqzBNV&X-Amz-Signature=bfd22b756af3c6f6da2113e5b2471e83eb368fb785184d20d0c1d75275bdc69f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

