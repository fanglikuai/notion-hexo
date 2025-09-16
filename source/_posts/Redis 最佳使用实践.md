---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMFBIF2P%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T062457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJGMEQCICRfd1ApFIYf20TLqkPQcgbaJ9kaUgKpcTTY4ldsfO7sAiATj%2Bgcg%2Ftyo04qyzWwnRTkFGUoTqrK4M1w19IVx3EAFiqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BtvShJiTePrXnSJiKtwDbV9KQox%2FRYDWI%2FNwe1r9kO71vR5K8RsBySpUFSHFiL8xmEZupLST861RErpvqfQSBXwQgTKpX%2BWE4AtJzRy6p2kpbpy76wI2R0YIBBb7%2BUBiGfeFWu6PHd7qy0ANrhZYco4s3jxdp8J7LhnlMKA2QhCOmY1lOeTtg1r7CkWuAgJ1BDkxuVppHVGHRyE%2FlSL2kv5%2FSWQ9JXVoaltMKrXCPFsBbKwekvjjH3Kays%2BaRbjGluQ3VtXi0dwJI8nRrtL4rUqR1cjsIliZ2cnb3BpYS8ieZR4426qoPXBNSYCylKfg2V9NMvi6T2tAVnP5OotyOrLliH86synAduwRWhYMwT8a2%2F3cPnsqNfOv6Ih6RbpzM4TudE5zZUZXnhyK%2Fg9ywmwhkbDGNtxWPfsaaUvUm9q%2B4na4k7I0Z669vf1fih2Agwx5dyCQR3COm3Q4P%2Flf9vZ%2FHZQesmXEnLpfoMDbHdr7rr8ndqfudLwy2Rr%2FBwNVHdY6jvS0DYznj6nXjmPreFRojYG%2BeVg2HJ%2FNVt0ydjL4Ddipep0YlCDm%2Bfr8BjNAXwSdf4Ax7Q%2Bir2I%2BcCQP86mu6VFE2z%2BGts7HC8qdY2FkSLb22HAxfEe8okcYZV482BG2ncrJQ95Yx6gwifWjxgY6pgErOQ5CR6cdTTPGpcSJZcKVIQZcN%2FzHSABFp5rOUoWzZu7GtDNLXziFfqZVeuQQnB7pxWcOpg8iiO14khqQwQC1BpJ7cVxNID9OrnlRjMGgRU5SgT78OfZXCaBA3DyHZamq45oIjKabxW6DubeVCcYSj2xUh7euu3dlcWcxKQMpK9H2qBznyhfWjMTwLYir3wlh9H3mXy4u3CfsS%2F5JaT%2BJ32u%2FR5ck&X-Amz-Signature=ccc82366f9624fff0aa522f11a6fd8f50b6d136c9a5d71f315972114c73d80e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

