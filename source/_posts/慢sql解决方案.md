---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673NX6475%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T160059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFrOp1Mf78ctew1oIpSkWJwJiamwt8Hka%2B7X51QmY2kqAiAY9IucjdXF0EY5Ozpa%2BVxdBEXBCnxEZtch3QEn%2F3g9TCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMUp%2Fni9%2F%2B7UEq0DEKKtwDWWi0Y2KkxGbUHCvTN%2Bi4eUkfRnUWIrC%2F3OrXwVojlhcVQ7F5B0I7UKANQia3AVH6cYzkqIvoBFjmav7YK9AO9DYilbsvVqWoO41Qy5W0wKgWA9Tmaa3mM7RRZ%2BRXWg2%2B6x8mIH9a%2Fy%2BMe6lHBEv8FMgK4IL%2BZHBs2ecW5pC03p0%2F6OXQygEAFUoIcyLkQT7ySaVOLx%2Fd1JrBuM7qyCJ2lfDYgSZdXfCL%2FGFuqwH5X4nx6qZ64ANEk8V06eOb687m5bgvSoxi5vDKdQCemSzF7fszfqGkc%2BLTDDtVrnasK8QelFqoM%2BQp6jccItIc%2Beg9uPRxPsLF2tz8IWulVJCfovMj6Lar6TuYn6TaTFr%2F39M3zbci1XCKOUrqcBJZeMFoKZ4RU3%2F3pK2EV%2FIaiLAcCifdYkRtGFevmmyGdB%2Buhsz2zSJtDW46PJaeQSWy1zcyEBmhidc3bjFhB2vJhp8qk4T8mDqqHdfCpWac%2BurPPT5ea9EuPZxaHa1He5IDnMAl3s76ggYMqyPRb0M%2Ftrw6kOPY2p2orFtqmVdRjcDnGyVYfLhaxpKrrNsHNnTk8dbyELnYByLUCpQjpl2l6NquLN9eA9F2wjgEIcQjP8UZrqLYdSmkixDKHvUudfEw7pzpxwY6pgH2JJ6iWR6f0OkGqmGmhRElZkAkpRSgydge1G4DQNQk9Ph7DjjibKRIw4006rlGADbXoGsVRzZw69AaewkiGBy%2BEaDZScj6XdfMFFSMmwUdxn%2FlTxdTTO4nMFKB7qIU3oq%2F5VaEM8mQbSw2dluJ25ccUdAoS%2FNDMURkwvfAuB1caF0aNdH3hJGhfuwd9T6mM5DN4IpAGYcR98xO3S2uZ%2BotF6oYkdi0&X-Amz-Signature=a539db443099470dfbfc9db5e249390d1ef307844ba9ffde8a4918653ada3618&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

