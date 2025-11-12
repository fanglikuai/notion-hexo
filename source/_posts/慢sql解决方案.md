---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646DY47KP%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T140039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDRkgrzk8JV5lJ5MFfSzQ30Q4nKax%2BK57YdcsAArm4FyAIgfN7jBY6sFV7TuiaMGTcZ9RJhCw73cZ1QiE6Gcf0A%2BDkq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDH7CQ7rv9fyZLy7oDyrcA%2BJvbZ2%2F61aEWZx34%2F1qvIAphOXSW1B0p5dN%2FDH7d1WlpD5WWz7LiYcoCvswQkCY4pnhjMkMVn5VotUP9jsvWpfY3nsOYFbS1aP%2B6my8gLRs3d2zpLqH1XlW6i%2BX3JwR0gK%2F0LwLVz3ghPH%2BDMV1KylGfFXPrmjZHsMrFF34WsM5Rmoxb2k2BNjtVQCE%2FvKkGQL6UtiIlCU%2FmZksqlSMp%2By%2FR6SRGSON%2FU%2F9vA5g5DiHCg%2BjOf6YGeZl9dyG%2F%2FK6ck0l71yu0UABaJCnOc%2FEeGahjCvkwD%2FR0Bwl7dqBHlwLkDDvTMyv7i%2F26DDvfmyOG7atLRSeVCKzI%2F1M1z%2F%2F%2BqY24zD5wg0XWV20Hpyi9kK7mWX6puB2Z2T8oOQUo3M%2F0PF8oKG1YvpaksvPo1Jv6om%2BdlFIQDUkjETHHahPN30qFyKKJLcreFdbThnaOcgtXNjWfMev3uxUHG0UQaySsySDwEUW7p2dpP%2F%2FEimZYbJS6j6DmJlEX5bl%2BeVRWF3%2FjaS0rQd2e9d5laDNxCR97hU%2F9oElKckD1z%2FXztyuU1Arog3tNVWghJhb%2BXm%2FvHssqKYLqe1QKm%2BB9YxbbnqrhSGkOfb1j1SwUt9Lcmryn8HE6D53Mbl4ouiNODgmMIeC0sgGOqUBYEu1ErMclZO4ZDm4%2BEAb%2FUnQawlc3MrkF25%2FqQmN6SgFYbLGIX96bG2PaSuSfKyFF1uhJdTTM1%2FxA6xIvH6a1HCOTnE8ZZp%2FM%2FEdMExUdCc4KHNSAxGt6S5hX6rdand3pvOHx2W5aO2rUmiF7JBpNE8xZXi406z1Ob4rTEtJvsMEwTe%2BqhZ2mJOOnFdr5USOOoAxgHhLjqyFjfa6uYiZmQI3GV2H&X-Amz-Signature=a7c16804ca9d8fe705ee6f238292a020fa26837ec5e92c9e4613c362d026d639&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

