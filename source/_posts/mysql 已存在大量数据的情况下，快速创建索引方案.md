---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2W3XRD4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQCrY%2Faf6nKePgTMMWBEoYICWYAPKKuLEVEunVucmvOK%2FQIgaLzFqSRzo0IJg3nLWlJkZET4G4AM35MaapjEZbakoxoqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFl9GPUQam7rIlAEHSrcAxSZv1Veaj%2FQd4ZGwj1VvZn%2FWT92FG8KvLVP2fTN27BTlcBulMC8X7a1XMlGKBkcZx4bhU7qkgfSy37VC4THiatQaz5a9HL5N%2FwuOay0qHjvfDABBCMoKNbDPtWeatCSB1JrLl4BJJxvaK9Mx1i9Ig6nF1oFy1H%2B%2BvgG5UEm%2BlFCKVpgPNYwOF4EpSh4ibxM05OQQ2n6P62rh6fCrwvD7sTxq%2FeHLweyFZznbm1An9B38p6ZQFZssLqQeCQzuNXZwCLV%2B5ClFZ%2FX49SvroXE1VZ0y3SGnohWtHdWV4jea7aly7MUu%2BY%2BZB85A7wcuiMqkpNAUNsgiHUzvuxhNinRZix7rP0sGHy82UCmXtvNkbSc6HlOKf8zI0KXqwhQAkbXuHG34vPcKK92QkFNC4YxeILh3hwrUByAJQ3fo8DIUGMJ8Ky3PteLK7XYklcGkB4gGW74Np%2FIJZJSGWpZY45lXsuKhch0qSCgBM7JJemVfvo88Ljnq3ngRbdCvGki4a1tCHvoZnE67J3UAkB35bQeEg%2BvfZt9Ke0hQTj5q%2FRwUxRqKc9Z3Lo8X9NkynjNwWmUBYCPWR1cgpZ%2Ba4VKibl9ewAiBe9LXRsZtcNWdw5%2Bj6ce%2BGkKMcSgY42Igj99MLHqucYGOqUBillWEu6%2F1Eyn4hCd84yL0yz%2Fg3CnebXFAWjDrhCRrMwXcfm%2BmNw1FQYK2%2B0W7GeoedhXf8eWrVHm9frB2s8N8uNKw2KkMh3bTN13vfu7ecmhLdedOFFi2NLnvW4I8Lf8yx5QMxmwpET79u%2FjvmAD6GjW9DkpBtrrHE9Y%2BJNI6%2FjkGlnZ1rMY4PDY%2B2ypSK9NhA7DRR%2FSfVuC1jt6C3Cl4I1U7RR9&X-Amz-Signature=e0c3d00adeec1b02702759c841611f9a19627602f8d04b26f3f9b1fa1441b4b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

