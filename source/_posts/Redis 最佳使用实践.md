---
categories: 整理输出
tags: []
description: ''
permalink: redis-best-practices
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THFJCF4K%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T105524Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKrFUnQl9vAYclwR%2Fw%2BGMdDGG2XvkQidl2pmOjdY%2BnLQIgJxTmbuZk7Zf1VYqKPNfbRlh6%2Fc1G1ZdKNotnu7IphzEq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDKOjvADhoV1IAm0IYyrcA3VU6kYetcnKzv6bfyWrcXoh%2FBqQnxs3X5iFOLFypO04CqBFWrqSNUqmpPeutYuEMTHZgliBBMTZFGw17ElJ0RSjBL5XPQg9Ja16GPelFvCWwPUsBE0IB%2FXvGs%2Fs6JoywGHeWx1GyJKJlo25KmQ1XhQ%2B0%2FnObmdx7hz7WPON4tRoXXAHK8juztfSKn60IBMQt1uSPwmgMrW0skedfi1o4T6o9Eb0o%2FJ8McGFGQm74XKFuZ9sMgC%2Bw8LBe0EF9dbVVCnjTSATdJDLW579ZRvRrhMXu7jSM%2FmWYfeIJhWcBLebxDx3jQ7PISz%2FEn%2B%2FAgVkz5v%2Ffkg8HNkmjDFo9f8i4iXWUvsrJNoogKpDsPnR2FogEqVQK6iubkSFC2LavdAccO0LnAHkje5bPdVXXCNIZNWLTeN3ZT6zVGHW34pKyI6YvGY2RBHNGvJVZUZHnCMKZz7q6zWGi%2BV%2B0TtAf9F1tZgnvE9qukQs5q6FQGZAW7YznUDJZbPP%2BeYHwW%2Fq%2B8CwR9jWoDKD3ueuIJJdjdk741MTBhg5JHbwtF9t3wQC4XWxLNmsaM466kAAlj3A80LNzxYuYl2xT4bOCvebcAt1mgq7fXQt5OE3%2BjNSC6ovMWElrEco3d5BU3jw2KUzMJ%2FPmcYGOqUBYP8V9Bw58b60rlYMI7aythxa5dwuqKggF7yj2pKsYTKbgedtTcZSRDOX69PMp4A6klxZL%2Fs8YwVGh5rwwIzREVxTyZARfg6OBMzrN9gX7zQc1BBmSeNMlXSsimoglmVIrPEkS3iWgPBxV%2FJuGgk%2B2Ld4IkEJvHubfxN5jRhu%2BbF2HkeaVxs0vkukdi3zTUEMYnDJcG99kCOQCp%2BTvHbM57NDtgRM&X-Amz-Signature=7ba8cdec511ee91b7930da22d208988f3cd41e3bc12beba7806f9c60b9036b36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 18:45:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
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
