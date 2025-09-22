---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OKUQCTN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaTj5avkVnCiEpuMu6r%2FQJ2AgICsO1LMdYp%2BcJEUEHRAIgB7SND8kY58V0Rs4uCZejsqM6kevLMdA%2F07zyXOPELn0q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDA2Z%2BTKPabZeAKLeeircA3Fa0ugxzP%2BD6iQ31rfgkSaN9CBwewslCFVgy1TsXEazu8%2B2aCgytWE082N7JgdzlZTUuLaTRmAqkfe8J%2BWbhy1pd3VfLjS%2FAfOayy1I3MVGZ9W617f%2BfmyLS5thFLyhCZjwOzZ3MK2ELW%2FOZCtWMiartc06eWKXwT6boYNA%2FIXUTLug%2FyrLRt3fcdGL80Xpnk615wV4fSlmWNFZ%2F397kld7Sa3yF7UdrdnyrQwGonC7as7MsP%2FXwz68soJRZQwFEYDarsGVJRjHr1ubJNz%2BMKIzuKdfFXCbEaOjL1PRGptv9E%2BIUUW7wn3b65Gtf1zTkWlOuq9w8jo4io3vGK12czjLDx%2F%2BtJxjKeLIuGTtf6m4go8hMGRP5lDl7YYbSfyqb6P%2FjIoPKLcUbHAwFYfWW1TCXSpDR8pCtIpdxnD5%2BrIzpYVGjv5gDMUMwSxtSIvA%2Bf6q0iz2%2F%2Bj6RQv5D1cZcju%2B7HD%2Fgf59Hwo6wLH1QTyF32dPhn2cA%2FgO54P%2FCvUTogs9MN7EuiFc6LWGoYmF0ha5viurOlPsK8Z5M4XxR8OyDKdrQv7oyDdPyn4Reo78XjfOOdWJG%2B%2FR%2FTeICHxtvWs45NAtX7ERb7%2FtZHAy4Jh3WRPArYdG6T3o5Cu9MPPhxcYGOqUBzzk5by3d6ddbm%2BFTnUtZMm%2BBQhMvE%2F8fD%2FpE5xeJt494BXTBembjgVnBkmDsLE9YFXkt1AKbtcKLIsKySeyu%2BNUhGJZOgEZcJR0rfhIGOG35OhUEqrMVjdeolpIX4FUR0DPNmWLFOStfum0AEXOlJ1H5AK8V9Buzr6Xn1WOEpIcWa%2FAYQM%2BSzV2QOmbVWgDJRq3GsuQqDlBPMgkaCOkGF4AxtoIZ&X-Amz-Signature=332c5549a68e0662235428223de2418124e18870c086e5c197ca8a960a3e0084&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

