---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHFLQDKF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJGMEQCIDWDAGgJNz2xKi1SAlMqHpjdZPAq1%2BkF6OnnXP9EuEHhAiAsNO2NxsqB0B369crqr8u%2BAmqTxlQV4ZiobXN8gW91mSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM82RPoEl9ep%2Fjvg5xKtwDBPDf53OhoDEh14vih8ufvUcue%2Bte46eReWBgh3%2FNylcXgGmSzjPiPYw1hVSZ3UMbopWwjyDedZdSABGWiKuvC0FnjR0mLs5WxxyB5kV82kCGSXvorsyOhC%2FIPoYMLL7cq%2BeKAxKjZZzKa%2B3NT%2FxBk82LhS1hCh%2FleI4GfmFv1UhCwAVgCMc2%2FOXdXRmzmRPYC06pcyVpWPhMgeFLdzmmWXKXZ7HM0%2B%2B4qbh%2FogzxVf%2FiDRcdyCAzMQYtu9XCc2OF7PBO4FsYPBU7hQjJ4bNs81A8i2ran3pMunX6L296XH5RmTY9An4Fve4mpJEN8OjSnDaPLp1jO7dE%2BlvUT9tZlOZvpGyHdEMpT4%2BnOYQdyfguuJ%2FgLpGYMZ8HCG5X4EueN11AMiqCedyIl2yERiQ9S5UlssxaIvtmpLOJURzK8ZLVDoiecWH%2FQmMQtlN1N%2BWxgvcZcCPV%2B9cbFxJQV%2BDjy6Pi1d1pzNnX1XHh2Y4Pp5fqYmCWtrY9vWXeEhgE3OldWJFqit7FtLhDWR6KG6c0X6OftLUcZhB9Tu8PxlU8VY4IiJpG7eOZnbUqQTdgNkB9LekM1%2FQa7AG0RdJOSo9Mn6de009KIGOPOX%2BNfsYi5CZluzQxd15urYR%2F3FQw%2Bp3axgY6pgEGjB6wcroe0uTXZeUg290rzeRZpiMrfyVlDZ6VN6TjSu2tlIVmrTwYJFVzEO2R1y3Fxc8OYB2%2F%2F5GKPQ0pqdfMDZog4NLljTk4n%2FAUbxm6u70FGcYNhMIRjsUoePksbto4vyOuxbzOSYSn7phOTRKr%2Ft5Meu%2Fu1b2NCqV%2F7Q7bcPl3m%2Bh7uLQH4rb1xI%2FGqYtLw%2B%2FZggEcXiSBMMq9lztHn8VnHGlm&X-Amz-Signature=7f27e829c9097c611e8bcc10d72043767d59f6a64d4c8f9e228e8a18d49b899c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

