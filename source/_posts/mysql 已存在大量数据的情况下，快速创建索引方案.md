---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF7GQOHL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCujlERI2c2MR2oU%2BtByKrtQm%2Bgo0DCjqqnKUKFIJ0ekQIhAKcGMWjbbFXBXDq7VVw%2BW5JME2RhP0Z1uYfPckAdJnayKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtS7kKMmQT87m%2BwwAq3AP1qp2GKgdjVvUA3EZjsIfEaom4s%2BRWcWEENvlqCQQeWcdVu%2B4KK3FEIqwO8aXDfa%2F8AWI%2FNn7gKnnhXIGAF%2FR4iTQDrxo9VSeb%2Btm9hN9nY1Y6C3xFpQ2gmJYJBznLWSPISSQMj9f0uUeDTmisvZLKXxOIOCtwX7eqWLUdBZDQTRxVIL6e9nPfFwLCzVZppTjR3zBKeMyBYtHr%2BIbhAhJdYJfQ2b1Ty74Z7XUBzQOjyEPfkcIYsoDXhTeTLtV25cj4DAdK5qUzrymL7GDEyrAyO%2FoSDtGl%2BxGhFrOQiAk0ffhM9cJrA4SrQXmLwGqpP%2FTbDculooIS8%2BemQEbhevfjzs0%2FsOcTFlUQQrVcTs4xL4EID5WJ9Agetlp2fSM1tWmY0NRMTLH%2FK0sWuX8jo7YSKMYEzelQH%2BVK2K8%2B5DVJMNLWhDB%2BH8dTEnFJrM8WV5ocQeKyhTVUT%2BKYaLDq%2B0uD%2FhSHHs0g3j4mPNCN8GK1zW5LP9KcKQKv6sOz9u3xb2FWALr7fmc%2Bb1NtNcZjSYOlQq3Z3DSmEfaf8icyH5nP%2BYND1kityOiLGkjD55OZQV7UdmVSoTHPWgqhgtJ6dFhcZEJmhNFSBZy0KHfmc6gBZm9093bSAUVM3CbiLDC0yYfIBjqkAc7UKs%2FckL%2BmP%2FdUblYceMktpIFA5ZnTP%2FSOzc1N26miWpOFWQ%2ByO0jFEd2gpYiCMuLksFgZt4P7Q3lqf%2B6ME3xGP49r813PLyuWRVGMQh551GJJnvKMA7fDrZ03zMRDug2sF4GFVUP%2Bby7YHIYzKnS4ymPJXEwYmvNURBb%2B%2F%2BmDe7LuLLE18xU4twUYFQdImbds4xS66n6cpNlA%2BE4BGY0wjGE0&X-Amz-Signature=bd61a3c53df3d573d99887165091e0e005147aabf392a2e2a98581cd00a90058&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

