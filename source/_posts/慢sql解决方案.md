---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJIQDX26%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T160235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIHYoWyzltOvEeGjHXUHXreOgDWqDbF7LByT3Kym%2FAm2xAiBi4KRJxfK1IOu%2Fr%2B8O9QDjdWKeIOrrWNmwx9HRqthhiyqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMylFC%2BY7ZRaDenvjvKtwD5u0gs9OP378x81jVt2F24IbHlKTR9PXrs%2BWP3X7UFHAlG6y3U0SgJzfy4iZo15MtD4KQDdcguG7%2F1%2BoHkvHppcu1nbhqul91XNV77FMS4mo0SgN%2FxpUbKKlRyKPR1sLnnqBLvPaNf1V2wNu2ZmQpWUXnnG%2FOqEe5kCau3w3cL0uRmHIXyjEj1ChOcub3RVFroGr1yuE8CDOTuL%2FqyNGWy0%2FWih5xINzBD%2BDE184HfLPraaLNqLj1CdvQbAegyjh4gu2kWBRMpRjsQil%2BKrY4UvKxbjbBmVbyy91jtpaBexiR3aYmKfdn2vsSLRPFb%2BAvXGCY%2B%2Fg1vdM9vg6gGo0gC9rOYJHqXwwQ4kTaTSHnX5TJvKWxO9hniysXq8%2FHgfSA%2FTRY64cCY63xH5A9A8Mhxjc%2FZDA68AI8j9ZCyHgalXmZKITQOxfcck1ZloGmwLoxmsz2CkPCu2QLN9oKjMvIgMJprCF9VdYiXlc34rnGut7dG1e2eCQpzP1wWpudcXcQ1%2BfyMAQXwDOkcCcyAj%2F2oVReplBAma7kyS3V2k4ASurjvjDaRWQxL1FuLkhFHWTQB6hcRtO4d4fTJF9IeJgWuqEjB%2F%2FX4dGbDwMXyDqfBEioABTdLVdjNn6sg3MwmpOfxwY6pgFgfq1EAnzKifjK1u3CsHG6%2B3hxhF3NiQ3Qh0ke0aLR8Kru2fwIowGCNg9iumWXCGWoUYaB0A7fd5jrapIpYk81Aasjw4bi1er4%2F9ChUfn%2BQ2%2FLcBKjgpkg6xs95s6ycLypDkSjHmun%2Fv6xXg2PRFWdXMPTQUobDjfF90IT0O4mPb50nBufeAvrzlfKOlS5cF5OmjdAEies4LJOYjBuemLzGTyYbdgB&X-Amz-Signature=157e7074f62b16fd6417d85f17d30d9ba237e6a0bce5aa1042410750b63e9b05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

