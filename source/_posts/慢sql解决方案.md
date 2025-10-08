---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCJ42O34%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQDQHPiImnpTtM%2Fg5Rc0VC2iWjdlggLm%2B9a9%2BtWQNQnZGwIhAOTEH7%2Ff3Z7A93smajIYtSbzcGtlmrH33pBOTnw5Hu8eKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEi7CHjElk3OqZ2Coq3AOHQPG8hUXKKfVLoZznjv5udelciUObwkYrK%2F6ZKVMFwCuFpMHyBshfqOxlIbIOPyxMv8xuherENTDhD8RqF8Lmq0BT%2Bczx%2BljseFZZ7t1DSbh9TQpQhKtYqhxr6Yy1QQR7ffrsn0yhZazciO3rwVHu8EprtI0m0ixqAOj3GOHS4AvCMJKZfUSEB%2FGY9PEOwAEl7f0DGQjWEGHbRibz4qaERsxHcCfrJLUSalJhFEITI%2FhGrLY6r%2B49JT3djJ9VfFXF%2F8eZhjUwt%2BRUR1A5fuGzTPqlaYfSf4R9Q%2BxlNdfyUwdw39Ln7VxQ79j60vdvMOfCsxG6vChjz1OzHHC3XCuGX5d6VXVPa8705k%2Bg746PRFKy5tXArseHAZybuDzjpUQT5DtVZAaorOdKkM8I%2BOCwLzAP2ZzCBQQFLzOPBmmKWeDK732ZaulhlHzrTwdvst93qGvht4xnuEMPmQIikXttFkX0qXcF5RNJqC1wplT%2BRL3K5%2BgxKn6XnD7c0A3Oe1f0hofk0WhRz8XO35fpLG9YXh4DLjMQCgS2nouHz98deERuL%2BVsv5qKSqYYrgJFUbJt%2FJ6l9yJ%2FrZq3V0%2BcUraJpuvT8Ce0Lf8vFWAWF1mxRcnxATeX9%2BCzky%2BtljClvJrHBjqkAZP1BqK59%2F4QtRtnTd2DMHCfTmXd4Xoe4zsFtyK1wvN9moYLaAMhwVsKDRdoWPENHA9b1TSWSHzjZSnddgUeiFdVVZT9QPJCBerkxpuZfrfDVqqBvf33FyTKSRFMxZrdoNGmQFZCseXIeFJfaiaTBzpW1gdbRS1VWESf%2Fz6ZjFeZwiMS4VlugI9lIPhyfmuMhpR8sbgH0tJ2m%2FS0P%2FOFXKJa%2FUcD&X-Amz-Signature=e84bf30462aa67b25191dfdc5f4313aea8c38cfaf03fa3cb301fcdb76ec25643&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

