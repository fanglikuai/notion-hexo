---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5HT43MK%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbN%2BM9T30f4dq26Z0w04RGUYapALhpuw4DXBIbs1l2WwIgNN9Qq%2FJBGeL%2BYouGt0S3CMomrACsr7yh5qKUJHagLT0qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2FH9ZDzndukDCfosyrcA2aVfoZsLzagkr2PTQpRfwP4aTutJXSPQ%2FYq0rGaFRHGvBaP2B6RhZzakM9jRRanEu%2FoLJGe0QKlx4uMcNktmu0xEsocIqIVKlr65ZMEzHDDj3qywJiJfYh9TFTTUWYvzJFw4XMO2uDPLwKxRUhF%2FSflfiCkXkcwjS1AkD4GFTeIRwoSQml1MuqmXEl%2FOGx6iOot8vcvcumo8r5UHFpOacPo9%2BHMmTbFdCy720pHkgp7EPEh29hPJDjqSpuHJLIjomcBAOYISuODobv1l3BDAM1OLtqyXADSQ4%2F%2B7yfWjvXpExRU%2BABavcG23qPHPJc3mYHX%2FrSw8XWhOWJZOShMx5kUxdUquNLOYMvVayzlbj7bQixLyK9IpZQh4fwq%2F0PA2gcOJUgM2KfII2Fzwe9NL27NtljLZi0YMCCx8a5K1sKWn%2BbfcvLF%2F0B9UE03aVFhUdtQjRJSsUSw6xXwL6klBVOogtnKNvJow9ct%2FGL09HhlcMhwIlS4NT9yGLWZFhG9cled2y5NUGMhisZ4UP9YqLvabaZFxFkjk3Y7%2BIO1I2zdbo3bwfIswT%2BIcpbYmKRhbwgq%2FKFx8pFgNkwj4%2F9eeYu6b8oxFa5jYrl4sacrXaMKKtprnRt46NAlNo%2F1MJ2gscgGOqUB93xLuemZOypfxfGJT93eG0rwqVS1aAedTgAp6c%2FZHFkTKXXogv%2F2y%2B0POTaVegm%2F373XDFMdcXWXsXiK2LeJ2Qhp9PDAUzYDsz8iNHl3Sn%2B0DEgHfGJQSGz3jVDsFx0lSz6NrLDo06e5qHI1VEhu%2BqVgm2oUS1GnVLkALzecJRLGd769dfrz9PQJmR5OAxeYYQ7tVgXS3pEh5VrxBbriNtdAhVtp&X-Amz-Signature=cdda76e6c6ab71142094d1d0a9421e86cea29f9a0620fdeef507740ad3e78d76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

