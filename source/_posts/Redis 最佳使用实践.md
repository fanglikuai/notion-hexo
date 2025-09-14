---
categories: 整理输出
tags: []
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZCCA3ZA%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T125005Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjYPByxsu0edPVV5556dqW6S2dI101rstjQPetGfiQ1AiEAnunqZvN10OiWI64tHl%2FxeDurWlZFGBVOusQIDkzYrf0q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDPXZmmC2dYLIpIjjbyrcA3jqw7epkyLYE8AT%2B25YkeAEfY5Qp2mWPQVRJg%2FfFs%2BDlHy2mKG%2BjUauPHgH8SpAEoVr4S3NUHSxYlrvQfbBMQ4VxF7PdZcQDAuvbUlgJ9yXlX16%2Bcc%2FQ77AcsTHjlOIXzYqTAApk8WOO8jo1SYWf68%2F7uQnXFFFfYJZiIXhFEmCht9cmTZ145Nh%2F1S0nwkPfZtNHt7Ob85jYl4twf41UHsDcSPZKdYPfmoDNLeNyyfHW5M8nK2gS4td1iedeeJ3qBOwttJkEBBp0L97SsQbd3z7e2Q9yvRaIVHixoJZbSj8s3HKHwmJ2X6Ko5PC%2F7eteJ2cstmynrp1%2FUiJjoLlTOWOUrFJGxhrh4R7%2B%2FYGRbmkGrJ9M7WIUu6x5fB7G487hIBjUuFkcOlJxfG4HXBUmprfbPPSNGOu%2BSexwWmHzu6DEY%2FCFFcus%2BJ3VhuH1xl8aOWUxyg%2FVg5Bx4gU6VvXYk4mxVWqEoIuVt18pMelUqRhlNmfVA1J%2BEuVaRVlMXhy8m3BASgQrM%2FuXangLJNbadg9BJt9k34rwi9HSiJNiTBQhYyFq46Tqj1ET7fd2bdGNvfUXO1UiXpomza4ZW2Coue22a0HU%2FpBMTijYAJ%2Bf9apW8P6XwpA1clbYb5pMJXQmcYGOqUBqdVfALQD51Ur5SQMOnHFmYk5DmFQA6CNjxH%2BQFzsE%2FXl31S4MIpWpvOiP7YzmrK5dc6LotChteTqQ5ReAy%2FTb7fUyLZa7i0x%2BWIkvpyUN7SeBBJ7MBgSiThp2eExKzxynGTlelZn91s4gc7Z6yzwHoOLePaehQSeC1jd3%2F0kavV6xKScAW7uKaGOldTXizTbtMf%2F8zQsWs8l%2FAXrTaOQ5E4uiszj&X-Amz-Signature=a5e4807561ee98fe8126f881b04b3bca4ab3db27ee5d82c0727a21744e908078&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:03:00'
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
