---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHZEUXMZ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5HiomxDeb1fMr6eKGyQQrZIsfm64zVy59rayqGt%2BolQIhANPOjWLMCxooSLwfOcfthQO5lHN7rO0Rq88spQdQGapQKv8DCGQQABoMNjM3NDIzMTgzODA1IgzaLX6YXYHmvpNr5gwq3APFsCBUvA1ln2usHbmSYQktzDqqQbsBAlb0jl%2FF1mCgVG9uJQ2VZVkvwjRsoy%2FKiCkuVchXBhXwIOHV%2FDKqgdKlQVbPN02EQRVf2DCclKeQ9nkI5i6fgh5JoKBHYx5OQkNPtrmSTNm%2FAtBAX9fDbMKx8l6D6s%2F7bYwR1KdQAP4mK2M6viuWn3xMDHqfiFaIiCJdE8tkXFOAD8lT5RxiFe7K6%2BpNtESt2RsCdEW6YdhdTWUK6HnoL19lxGpKiWPI8nktVKO7qQqOy1XK1EAMdmIj3qwwXQFnhJT8wJ4civVKDfWfJx%2Ft3UPqfX4JmQtwymgX0uuqFEGm4NqYQLD6KFxKXYMdxK7%2F3t2zewTfJmJ3S3rvqpY7SdbAOsJoLvuDwnqQMFmAroAif6nK3baGwYB7FNSRWErKsCUnbxt2UGsCBJWG82U1JP6pbBBTEdt2wDUrPHsJNj%2F5eNaf33TdJasUkeg6MhM%2B9OE8eKhVgxs1KydQXg5bLFco2uk5mjT6sE6tjddfKM9aQyUmPo6zXwvI3cb9f8aS8cjIKnqxTzhbeAm60oLX8E2jugaAhpUzSXXg2TvJLcBew2lzMJtHPct66cFFwAkUAS02BDZ6A5GN4tGJw7VyENflkSRvCjCq86PIBjqkAVAeoAuDLVycwWp0ZzX9Vulv7lpLeBEQEshT5Lxs39dm6SuJNaPIH3d55HV2dfuwx%2BlZ0KbgTEcKYEybpQvTXeeLXQrAKPPSVoO7WhrR4JILyGkVpywZNfTibhTdWgC%2F5UjYsg%2FhSpRkjFxfqZgOn3V8LgrW4L3k6stlZ6c7%2Bi9MnaxwB0HnsSypjLcUaAv96F4ft3qTqlVXbC1389QNZ7D1FODO&X-Amz-Signature=3b2a73b2c25088f63dde97d33f1ec96590328b9166f5b29c91814c34906fe90e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

