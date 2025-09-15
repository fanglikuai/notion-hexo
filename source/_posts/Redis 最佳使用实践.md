---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WY552B3L%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T153712Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCv0Bahu2zKjPQax%2FxWWrRag1ME8DyZzKmTWtKQRRXRPAIgf37O%2Fhgdl70yHHoVBFZfEItZgcf72QE6Jc72Pa4FEt4q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDL9VoJ60fMA3H0wZpyrcA%2BQFkO%2B%2F5PEHboGeno%2Fevi3X5Ay4yirBAmsNz97qO8IGbX4tEYG%2B79F6kS%2F9UgCnk5F4F%2BTLcZ1SboH9x0k3qBgjfWljLuRFOqHaEoNu8PsQEtwPsALFn983kCglc3qdsR%2Fq7ZhRY1NYlONJfS3m9sUv9KcSCjpqlSTZIQPES9PI61PSAWKQt7qapcMm%2Bo6mMd04jXxEmBuBqzZwdtNhhiPsAOiBG7qzJHmFY%2BMD0VEsI0o5QQnRa4fajlVybVo0d1gIg0YSpEHVe60wyewnHg0H99dgvFYfJWfl3VjLw0FE1Og7gViP8Jwl2Ih9XmV0Zv1prv8MC8a5xq9LJuHdI0YGhZ1vG2kOWiSiz09bXSYu4Oe2rSSEcobcX1jEe%2FqZ2TzylYt5D3OjQ22vObAnlHqbGKQW9toaEfPS%2FtjC9e9g8WFXKEdA0kBhjP7hFsKjBZFBFEvNZIuAoX0c%2Fdv9fujiu9mUSQRf8i6mUo2PJmrDUBWNR0zhyQ2hIBLoRQBtPHcNXdaXOhVOmfbPCGgSW6bKKje%2Fxlh9nswt8x1TWAErhHXLTAfT%2BrNHhuBsihL8PWQ79Sgk1qnf6z1wTPsDKJgfWX3WDzkcaQ9OLQaesO4CP9FcPJcQu0K%2FelIYMIbboMYGOqUB2SxFBouMrS4VLlsbe7jDWUgreCbOnmcMs22cCD5KCPm%2FFx2ct5tkd4nTcwebUHB8kaoKg%2BsEvHR8l%2BJJdT8zfattjvy%2F8CwUbQpBW9bSXl82%2BZG%2B0Ju8LfnAfbF%2Fi0LiZ%2FyH5xu7Igb6rH9IHT3LABqSRKOJHvvwZrW0iJeA6BlcOR9jzhOcU2v7PhDmrtdR3ZGUiSeT1XgpvQpyA80daP86to9q&X-Amz-Signature=5103904c20f5303f50c5e904a7aa045d405c3ce27f96387b87e1a1d5f733c68f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 22:46:00'
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

# 大key进行拆分


todo

