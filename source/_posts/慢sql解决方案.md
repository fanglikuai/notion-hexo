---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7KYLXRA%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T150106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFJ775pVeEzvNCcYiXKAQPMU6tBICbDg7CH0gtvssYF2AiB9aR3p3qnuXaqWhJmWnW4O3MjfMW3rVZ54PQmqde7wjiqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtM8IlNkhBSsw%2FflEKtwD88ZtzNlrO5EWkk1iTy3xPDmD0OGQP3xBPfybNoPUrBiRuYwQnP9HLo2w0pI%2BjzBzf8iQPuwwgr7ZZep7dFiNeg4mgL9LlfH3u0OMmU%2Bt85KmEI1i4la1YR4KrEEoFDU9FJ2NkhlG80YoZwBr5l6kF7CixF740qUu5jIdNLcRKoRGlldesj4xJ5k3F7FjGd98zAkl%2BZKIHhtZZAOa7%2FjP04REwAG177p%2BALgiEHO%2Fw6YV6VgSZinrxk6KRITU0ClWetBzzAGHjxzMXedNxzSOVvAyKKND5Z5K6Mbd2ojeOID0vB9S%2B2NWNMMMqJ6xVdTGdxEFEhEtAyAuDqX0BUkEWMzHI1HXBmujnam8pBIQNi3F%2BAvrsE9%2B5LTAC0ee75M76Bfjn0a6fUVA2oaa2V1PdN2ejgYb%2BSe6oHjKOArFKlGzPacIKncHaRGhMYVery98y0CgEpYXf%2FjL8NNUGsLmfFaBGYlledm6P8Wf6LFtmTwX3UuOJExqusXeDpJjmS3RJTQzBsJG3YWX2SiTNp%2FYizTPHMf%2BIwlJymj4kJg1TgOA59NE%2B%2Bhxd06H2QgXZDj2zS%2F55cEbjjQ7nGAkb7tgHKqqOzG8NtkG3KjEXBMRoXB8LfTuJWU6nZRX%2B8ow4%2F79xwY6pgGdBqKTtIym2OVErcwV5GSQ82YHiwbBtH7K4KIiLwx0qq6cSrzdR1M3ZbOW25WisTRVaVIxHZSISe4kEzBrJDO0upDFZgqwGnsYAJBvlxn%2B6mlJld6dJNQ%2BxgaPdaAAl76JeiP7afgY%2FSxyooOoCCLGh4zVoMXUHro8OR0duV%2F45oq8e5vphS%2B17joqSHFEyi9%2FDQdUL6EBAr4TtDa%2Bqgl1wtLixqQ%2F&X-Amz-Signature=f69606258a9a18a2ca01d6823f276bcc0c0215d025f674b3bc54bcded50a4f91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

