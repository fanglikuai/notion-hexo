---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XUQ3BX5X%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF0wehRpAn7OS1ipa1%2FOceWuOYZb7OWDm5anxdwHgW2xAiBycxTGcmpod8%2BfSu3150sK8pE8UnXFhlduOgfCyU77PiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyxKOjNlYDp2A4w0vKtwDN4l7m9BUSQ%2Bxp7qttcARBnFYqKprKaqcIxgxMJjGfIqsgAEdlz09iQsywZ8LcR1nwbTMJcA%2BvCy2%2Fq%2FXfIiRC32FWogsIakxPqS4YrQuDGxSayKPmwNAE7KwJMjWiH4mkB5BLgHKg4fhFjUVuqmP9FMp1g0BCChoG14cWZxh8CZHom%2BbKH0g3TkuLfMM7yUeZSkKo0%2Ff6Q3MxArirbZ7GaTedZLTADMlEUTz1xzUDAhpOZU6EAZfuVD4OjaWvWlIQqfXtZVhAKxY8pLQijFQvCgbhIrJiwR3ZRagHuQB8ZiV6ZthPcaKD3VDbeTbJsnyqf7NuFM6zsl4%2FC7%2FBVVfmwAJZBEPhye6rZ37DqApGOsVh2g24KwjoFFBvNg4K90ATsQAfJLzehc%2BxH7dfKUXykL5wxQBc9r716GMG%2FYl2qT2yaAKwYFb3gfI6RlnSGEz4v6uLDBo44gSMZV3rZwNZb2qmU508hzKx1a1pZMimcnaaJfkFbP3HdT96BSiQ5n0kJqifgIK7mP145PbvCQhlsy2MUdGna%2F6cIzk2tbNZ%2BYiUDNUuY487Af92nI0LevA64JvdEevyfyH39Of1H1Ejr%2Bz7YcyA5Op0L%2FHfmnPwzEEUbIhGvDfieZH5Zgw79y1yAY6pgH8efQK9pdPoVackfSIEq4uR4bErKd1JQxgHqkpOn6qmCwb2H8k6S%2Bdj0zmA8sHWLlrZ5xd%2F5Ka0X9iPUhV0jsGMH8%2F5We29AdMUsJz2Vbpz0zUThSvXJ%2F9rrApizg6tAYrPkdWYz7%2FQdJnIokm337DXqAVhVblKsrLLO6uHLbnFulq8SU03GR8twWXKVscF%2FLcX9ceCyK2oi8HtXFS5egc8FJ%2BmDZA&X-Amz-Signature=e6eb6328c3d4f7f44fea3b087f6c588d1c093854268e67ae768fd7b9b30d3df1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

