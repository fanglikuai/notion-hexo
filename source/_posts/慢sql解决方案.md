---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEIOPD%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEcCFbTr9ekUX2i9WY5M9aGevPNTB5H0Tp3KaK0W%2BqkVAiBqg03V2u39iO5fkjo%2FQ1s7ivh4WVf5BGHXxAwFtrM4VCr%2FAwhPEAAaDDYzNzQyMzE4MzgwNSIMyDIvo5E0ygU0to2ZKtwDDrMAf5sWdD5EWgCeZLv5coD5UQ27FZtISgxoscikwFP%2FegjQitTdbxj0xyxezhyI%2B6RFvRxriyFiHCmgrCO33ZUjT%2F0QCnY4b2ENPCDrdQrTU4Unf9jQXzBuCIPxbZsC8HNjMN%2BNFIpYZtU3ZH0MzbaA1%2BMZYspj6IpudQlCpxddCG20keLgcjM6nkIjPAMyX8RuNhri7zGT2AL%2FZ0Bx2EpaLhJtNYfRr%2FIolI%2FG84DFkKBQAFM5q5MAr4%2BXWbUThm4dKNLNBR%2FAAV4PEBOC3ebBpKabb5xAY%2FdDRzK1ecNX3W8eMDw2%2FVBK5T8snBruOUqJzKcmDoRTPBMd7DT%2BIq86KmFqqwquIZjKZJloGlkgJXuXi5caZCJgToPZDLgb%2B%2BcEpO9BIoKVv7Ojywhy9hUE4l%2F5o1ArwFKfvp7jHcgWwWjszvsSZJIMZPMAJRN5VevDgkp9V5gWOzTLFitcMCeZblXeYjcitCjcZ0Yiec5BpVnGakU3hwmINjO4v7NFxoW0unZNri%2BRrLcHxm%2FSbYgF2x1eBsg%2BAZghKNQxNyUmcsW8j7ga8UnJJMrw7Lyw4epsRGQ51liOTsx7ZsXOHKqIa3pZe0gEtwB26xQnZmY6%2B6gxn5G18vZDDjMw6o2BxwY6pgFtOFM80Vqn8J1px9MeyvVWXdRFDC9uMIoZeV2idJnv71UU0z51UaqCqEbWoDJI9LafVPxE3AZXU6zFvFlaRaTt%2BnuQqHkJmkjmktqzUeJyvvQFJiF9vRvK94%2BAHMXQDK%2FEmOxWvcADSJqEqHahsWBaHZFPiwY1JXrFrKuBLqHm70%2BTz2XL6rMigsZVZXRdOukszzNF2gMkHE1IPKt9C7J6Oh%2BZIop1&X-Amz-Signature=f0c1029e9ee5aadd4fc0df75b0772603f0d5409aa2e7e2ac4d71d47ede0dd9c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

