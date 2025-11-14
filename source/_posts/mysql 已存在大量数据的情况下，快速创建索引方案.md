---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5ST6Y3%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZqO%2FjfmeP2YCYtbLbEqDmFV7XW5PoS%2FTzsHrxarF4wwIhAKGXiaX%2BVQrOcLe%2FBa0pKfpxxka4kxpVEelF2I5id0mFKv8DCFsQABoMNjM3NDIzMTgzODA1IgwjG%2BlEbkdj8079Jxoq3AMBDBbj9Z9IwbMew%2BhSnVJFnws5Mii6lFGpPA155dWdXoZ7z4JTvYf9mQmAIpZbwldA2vJg%2FWgE%2FTD9lmbol71YqVha351%2BPz%2BFhP3CwxbigrfWtgoIkQ1z4qgWA11Psim%2BnUpBhdttSPVcd60Ts7kZiDKzaY2WdlM9XoGhRK8INCW14JQtifyhigWI0MzUrZ%2Bk1Px2GAAb4s6THVviLk0ZeDzyq%2FM%2F4ATagskzl6KuLi%2BZWTV%2F2KnvIbKHK9w%2BcGZouPv%2BFw9lH9k8hc88GMUFusIegSmuLmfEnQkxdliXhYpecjY9ok3UNGjf0I03%2FnMRjTbYdchRLTorWxvmxnBFfAIWnoVgXMyq0eGO41uHp3tdgsjhnwHFQm%2B36Of73HDyNBdNg0njGYKbVI3uCon1x8rkIsn7zeK%2Fca5y1Uys2mo%2FK0Igd%2FM6xPXPzUw0Z%2B1i%2FKoypxnZeIxsfbOTC5HH8I2ogQbr4D3NDjjpNTRkPtCZ1UfOe3rBXYYQRGoy7QjdifwksnUc%2BmdbXmJ8jOmN%2BQcBvhqrJI32ym2AQPXu5KjegaNyTdLooubHgeFkiaBLWvktLQdfXcUq8V1W5KRzPJv7%2FZ9tdSRQ58%2FWlG%2Fs2l8rYer55QXuR7uvqjCVlNrIBjqkARfMrphGr%2F%2BCeYy409vuR%2BNFx41p5jwL6vX1pFZ3sNMEYe6%2F%2FuHnoEXYeDuOwUAXcogaQ2fX1XwCaU8xrbzT2bZ6LvhNYhxXHXL%2Bt4KSGRFp4ZaWfxfEJbgcGjX9MSrMkUAj%2FkOSDGCZRlSuRpNFOuLCEUHCGuH46Y%2FQ8J3tzSpGyAyA2suqdYnsRAHwtAlCydv56AsFqgjQeEKsZoU%2B5gMfIrh9&X-Amz-Signature=77a71c503f894bfa4ceb9da2262ccd98befa023691908c46ff2b9b1c302bdaa4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

