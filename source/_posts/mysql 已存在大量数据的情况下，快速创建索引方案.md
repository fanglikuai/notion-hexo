---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U4X4TKSO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCoLEpTkBYGL3fezoB1RMw8xa74zjRltNJEo0BN1GFHOwIgTH5vrWy%2FZ1CiV1WzJvAJFJBoCpcuddY7PWlkMr2Qx6sq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDFEkEGn5a4XxMoshtircA6bDljqT8vADuBECZy3ertMJSgwyWn7JrtNIJw5defCCksNgZ%2Fzz264QploE%2BOCO4bmcwtEOeiDb1dgd%2FKscSB8dAuud%2B7XQpAdNB07QPsQQpJ0T1RAfTiQbmwjKL3EQ0sO0dtEjGbxCU%2B14QuOPp4zuwMy32v60Qd8B3TyHOPlPynEmaqQHq0oqqcV3ploYVRJ0VAmNI%2ByYkeBdM%2B%2B3Dyad7uBaK7D5XYQBAsGKejWBFthm7vo1K5sqkP0wuzrTTE0HxmKElLogtgxnmmRRPwXYKSPNuY%2BK30h0kozZYSh8w94WagMGoxFZ%2B%2BR60z9A%2B6fMjwXHDgifOISNycqp1o59X4l2HEvCGa6ayDDNaQiPpmg42gBbDK0Vohp5nkPe5tBxsuOSwRGNqx30HX%2BGyvUaJ%2FMdJesLdApWTSgFq7hZxBjd7IKEWsdJGk0ybdC3D2idNfAZxCgwmfTVzXVoHO0YCglazEFvqFZXEFwHXjcrn17hG%2FsSBiP9v8coKkr6EXoL4Ymz5rkGjriZEzt%2FmTxuxNhmt8sjlEPiMXpaOW0heocRMmNoTbiPwgTjuU8rp%2FZlhqi6I4QO1fA2CjxssEq5%2FNXYZFBg7Ro7UWo%2F0%2FrpcdPMlo8l9NHtG4luMP%2BaiccGOqUBRLJORkmsdXCnh5QctqoX2LZbmObmZFsEasbJjLjYmoaE%2Bbd7tBZJaJw4j0syzMhIejT7LOhphY%2BZK1MofsPoCHr6rv3NHaRSWZa47w9VdFkGCVXBb54YlqQ6kiczMMhgMFgDeys55%2BXSDygqosoVsEX0nNMWymdCzoVnp6tKxhtoN6%2FiARFFb03KdKDeoE0j4iUtt5ZRAx2WRzH%2FCf2I%2Bk04Wz3w&X-Amz-Signature=616870c2758b92a7c46305840c7c480e9d96f390463e9bcd81a17ebb808c1c8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

