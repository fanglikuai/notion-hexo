---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DZ3CB3S%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCWspmqhWkhP0%2Be0DAHatLD7D4tFebOC4o%2FL9vlZA3JyQIhALYJCY8a%2Bx1Vh1Ff2blB12q379hQzgyFr8XJqcQZLrviKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvZzlPdE8q4VDyOcoq3ANpXmL0yicAU42d0T6SZ7FLclvfEpADU3sXZ5%2FrIZ%2BA3k6%2FpMDS9fC6VhyxqLGnEjBXK4VOsHtI2ayfIS4mwahLRWhRWVhUdRq%2FShT1N3wpJotDBfmT%2FhGMbhkHMtVqs1YpM5QzLBvWzA7xdpsSetVqXnCPQyT4CgANtYxxXXfEBFYl5exzxIX6bgKN0p%2B4FYKZKde7noa8BAirxQ2%2Fj3JCyIxkVmIr73Rq5248S7Ub28cC%2FWvXxwuj3iy3A%2B8%2FksquhlHHdb5x5jZHQfEEYm6LgUlwaxSXmwmQoX7Z89s7CNrrhEosw3IZwqPSHc1PrugHzJVUd2F11psDGhiXB8Ek6RaBIpa5TUzkqSE4wFYHUwrd0uqk2vHC0t%2BSMROyS5hWtmqRonsAELDnuKrZI%2FlG2ys9nM2ea1smIR8UYAzWsZZng1tRLYSAsWS470GycgxcUFNuDiWjgSzGn%2BOGKgK%2BQTAtfqAN7J2xaVuw5%2Bvrft8Ofo%2Fm1XkoWZwZ3xLADppPXIzrenv3RFvw9Dz0B903USD43gY3bDCLEMXDgePsaOuvNGUxMFJHxLI9v%2FbvVOS%2BXyblh4%2BMocWoQnc8Xs4hEL9KElsIAraggKx8nzhCjaadcGFlmvSE%2FQXnuzCGhYLIBjqkAdMsc%2FwVZcMKMOneLVSG4aCGfa6zHbwnRuC2c3xb1WilLa49bkLKoZAzNP%2FXsw8yV3C662aRSy%2FcQfsZXvBlJ4OPeX9eTQcPIOjfkerM9Gi24agL35Awjm3IvfqAbFlkTfuvnMEhBUBGmQlsPDER3rtcqTMgS60usfkkWPSW%2FLSxd2651CBjhhhBxmQ4beibmlfEYZU1KVQDlK1TAIRrpB9tBvlt&X-Amz-Signature=2fcdb0c2124d6617118b6eba8642da7bc9fe76807da964a09bfcd41dd6acd49d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

