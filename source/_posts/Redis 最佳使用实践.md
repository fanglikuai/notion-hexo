---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UEE7AHP%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T142021Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAUE0XQL%2BZOrHEcg2IxTWDz41IlSOOeH6TIj%2BJaj%2FGyRAiEA4lfmJp2OUatNeduasTbsRwQNFBA4%2FBmTmQE6m3OkWQQq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDGz%2F4l3OFeFeu70zJyrcAwtrlgikzcImovJfPeCYN31MvTpValdgTcjCydPWTQOlvPnTDR5CmAzm9LoYA2XpwI0UDsnvsJiBBxgtB%2FaXl2j5wam9yR6%2BCKPnFIrks%2Fs47dF8eMUrEltFkFJFkraNQTkkeME2Ddbd6T6NQQ94N8vFPV4tmnwGPxckBfe4mIBq27dyYUrOxkSgIXkcegOJh2NY3h8GYdqRISXQSrYtjJz2q%2FS%2BLcYFyj3flw0iGnM5%2FUHsIelnSfBJ93sOPAeeshddheT%2B2BKmsP%2F8r0fY%2FdV36rm2EoWDkchSKGata4oKQSyhGKZvbbWBS9abSJ5sYHsHtEBw5Y2OovDLmnwHW2M3MAnIGLvr7ONvf%2BHR5tYH0Sr5BLyItT7pe1NJT6W9qLUD7Rsaeq7TkcV2Botea5qJv7aScnASw1%2FzmFu2KNCZXjmlT7%2FH6EJHFMw3QAKrIJmVDE9VzqooSRUXiZtRvX4qeg4S5Q1Cl7hfTdqKm%2Fqm%2FmktXuvvcjZIsgSC92FIJmJchEfR4eZlXJ%2FFAnH2rE6MHK4utPttm0mSA8gqsYiV3Vt40d7cd064wG8qGWG0jcjxeLuaYuKoUrkAxMTqlgf1dtWH%2BiEl%2BzbOVZOnkPmWTSmW4v6tsiGPGADFMO61oMYGOqUBkfeNxqvZ0p2b%2F9HVs9elMMe0IB%2FiNcAtu4R%2FC00%2BomoS12HIKpA7cUu8RogZf8r8bO%2FShUB1kDJIUlNOURbo8onzBBru7UC5olU1x04WaLiTsiuJBwTMRNXEEuww44Pl9FBUz4ssxmHQp1C26NbYZcmATB%2Bs6BjuNpQy%2FxQhEiuGsBHA%2Bf8RbvOjHTEBs6s3BNXmBjC2J%2FJvc1zpVJ5HR4LnnOrW&X-Amz-Signature=fb8668b23512017f7d4f2e8c7b55871a5ab8871e786fd9121120fa69392da489&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key
