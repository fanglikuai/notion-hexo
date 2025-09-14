---
categories: 整理输出
tags: []
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ITFFWAR%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132915Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHkpcQSAPk6LhitzBPutn%2F0nnIF58U0qjORCcFUHi4DvAiAf0XB9CdvQh5f3srDRG40hmcoHGRCdIV%2BetXUTZ7UyOSr%2FAwheEAAaDDYzNzQyMzE4MzgwNSIM6N%2FS437X1sSB8ZhhKtwDw%2BO41Ta4S%2FtccIpLgWv%2BafiL1Fs38WFh99ICyS9vLClLvGo3%2FuxKHUrhtwKHHnS3fmQU%2F%2B3jT%2Fsa9UQf%2BpNbck1HqQ862OUu8i0qignWYndwO8cejkqUrI1b8bDoKd2YgSl6huxCOt4qWvPSsIbYcHq9CgBxBUX4OVLtZSKwqcYkDrmcsYxUivEHEhjJrDkfgZgYOhvAe6swuEksc15oRLb56vlimZTQ%2F%2BsPVsXIcxV1PMlRfOigxfTfir0kvzpfRaHWPKbRx9kCTpTr8WpsHv9WxfpmWcDphnXICimxVS7CQMdJ3zLyhT3a67pm9abfiwCZlCs6TFC%2Fdwv%2FK8O5NIwaJ4M79lhy%2BPwEIEKX58O8Ppl8MRZBcACWrW3ZTmfU%2B8oqiXLfIn4nQLLNrtwUoDLzAVWrEfZ3RhYxOcAQUQcH00RHUuTwWabvvUvTiKLPHeuk4hSkUCl2XTu5hn7en9okqLLTHtrmMmbIg2WRPIgc5RYVEAulAvu1IhrQpw4UlWemhVcBIFjPad7Bi2Cdl%2B15%2FbPsPcgPCWup%2BnqJ%2BZJXFFSWfAaMKo%2F2dBN8eIgZZ26h%2BThpuXlYbgvW5Tvbd55M4DaUL%2FbKJkUBcLPrkV8Vn9NrfLPANzoSxcUw7vCaxgY6pgFL6S2MbXuEVybam7Z78bye%2FlQebglewD9dpNj3KQzt5F3FvDFVSl%2BNxWrxKNZE0pCvGsbs00pLSkSGJFe6Dej4uTNw%2BTw79kyXjXbqMiyqZSZ65H6%2Bm%2B2xdSUra7Y0%2FdEZZZdK4SSb%2FbCcXy2sSBQgHtlsLoHxsEy79X6L92zXmchyHx%2BP6GXJQX1zQKN3YUZbCb6OyfiQ%2BZfOcB%2B1QFHNtgP5VOZU&X-Amz-Signature=993c6bee3937e59b2cdf4387f63a809f3c8564a22c6e20b4f9e091feb5da068c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
