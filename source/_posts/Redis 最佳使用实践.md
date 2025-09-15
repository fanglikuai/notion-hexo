---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCZAVSO%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T081019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID4X%2BZTKiResaPMurPKYsx7XqPpWnBqj1%2B4VEej2a%2FdsAiBd1ta3zo%2B1VLdXbIRpumxrhAjKGOopcEI%2Bp3Hh%2F6qDzir%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIM6nGuRe4WO9D59n4WKtwDPSCDdep1nYnjAmGlFw%2Flm9ECrbMUC%2BOZLDhE1%2Bwxk5OW0nt1%2FhU1HhMH%2BooWAZrndgKEs3PoQZruAHZ%2BE9vPTYxbf4L%2B5Glx3cRWANzMeDurEQYQOYhZkjH56hjXDujwWvK%2FvWv5QLWBcHFsIKE5cCA9xkE9jAbUOByILvA3ttoPrgpBWncagi%2BEQlbvqGaLcAAjDn%2Bokq0kq5bjKFbHKFZqEszZlYTnDC6lGV3bl4oPoZsC%2FCkz5gTFHWVTP96qSMeIQHvRZo48y35LcWu8LqhHmWEBrfJezLW%2FWBqsUlrVZTPdA9wOmYBWJTHsWTDABG%2BTPNIvqPycPZeMrtlf7HgS8ELcHvGF9%2BmA%2BunTK2xYIkOduoX6dgNvslx6madqRypyYrK26MOxenbxZwGNPkLWJQlp5Dt0jQhO%2FiQEltuOqglvJgeudxZVD9V7Lq%2BbSuDmCCsqxhMTnFr29gkEXaBPXfsDzZShc9N1%2FiggfPUtun0Py24E890fFTCYitFHff2bv0nhY8dKy15D23yZtjc%2FC%2FbBOB%2B0%2BTgHfsR4wQUut6AcMmTemKEuPnDScEAoIGwsp0fpAk0oxi28XSHZy%2BkzyROmA%2F297%2FhDqD%2BhdLrxfkWP7KYhSnEK8q8wp9%2BexgY6pgGDPTINTNBTpMq7e0VDZ9NxQThBVNK3o1XJmt0HSeMDCWmmV1vGbeJ%2BKSvtKAV62JrgYAqfP6kcCsooOMwvXlD4iSHws77GYquljZB7SfAYDG92qrWlpr8RMQ1qWlhFxH9ikAPVm1NX51rOv4bMU%2BDxfYXwVXlOnvca4%2BcwjEFABUoOz3ZnVp9t44yglT%2Bu6xEBHYamZeLeIHEPsyawJkbK9KOgDUoY&X-Amz-Signature=8421c0ea0bdbb280f39fb1cee58b5b18120718d01c24eb28932aea3066291751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
