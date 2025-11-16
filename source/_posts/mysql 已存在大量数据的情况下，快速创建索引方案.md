---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7UW275%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCXcnotzGQXwFAFoYtvOOXVJhZ8cUJF%2F2EjSERtlibGYQIhAM7CRf2QIEeXoYRVaBXloRqi2Ft0%2FK4T%2BWAwE7xfnelMKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp8pJ6mMjYzUV4Dcwq3AMXz0KCknweW4tVKu2ZJOPRvlp4yMHsso4YZcXBC4h%2FJpdR5dlcAxg3MhilEKgs2g6AHGS9HS42lU2jrSDit3UVZKHONmCvA%2B1wF8ik1dv2DK2qcb8jyhXVCHtBYhHUyEKNnnTl8iWENHrWW2nj5vHdyff26Mt472Wwq%2Fi%2F7plL%2FYxIoL6c4zEmgT3%2F%2FL9pAa1eOY%2FqGW%2BfZQv%2BG8x4PrWpJw%2FWRat0QCTtTRIMaMxh3y46q%2FqL17rU%2BrQp4JhK77MUj3tO7fC2F755%2F2NTJB2Dh7yQ8hWstVIPj1Rq57phjvK2xIF6OLTP4KvXVJuFzmqYFSLEoP%2FaBZl7AssUCyxblDAa3CCTxJjFxKWdmuXcL5e07g1MzDn7JGEh9DPH45CACFkl9MOjDk66Zh9M8j%2FlNSdgtTQZF0%2BbgP387iX4vsZOYPE6DbMVXGGF8b042Oa6xKOI6PmKr7VTISHYjJbjvnV%2BKKJp0TKmmJEHkzufs4yM4JaJrpYL0afuo%2BKrnvjGRSPWSakdVjC%2BO4zCuufZupO%2Bmc95yq%2BCFXW978m43wW6VYpF337F2L%2BDIxuaPt7sd1bQ%2FGSw2jq1L4bhZOCgN4QELYZV7YakYNdroETP6xGEepzfP7S6xtPE%2BzDwzuTIBjqkASvdmKNtE9dKgKEKmJwOhl30tT2pKA87LKfIqnlWXhA2sfZoPet9POsTS2aC6QJolmrMKX4wSkkyaBNYJtFi1Yfi8m%2ByD6MqtwyHB7qzvkCh%2FeuJ7Y95p0dpArss1cm%2BeqhpESPnCxcGyG%2BQUbKlmaofCopXFxJtiJ0%2F%2FPq%2BjhoAA2kozc93dpp%2FB7k79jeeQWIpd5%2FKzSLuNiKsohGmRSuu2wCY&X-Amz-Signature=216421acbe6295877184638cc72e9bed59ed08e01b1c0950a4c7aa6491d656f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

