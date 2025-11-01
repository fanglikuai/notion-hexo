---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622LT5U6S%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIFRmdC6AL4%2FD3%2B%2FuQcqV%2BEaOW2aMk8HL6KMA5eeG%2Beo2AiEAsqdXaJnGtu1ZZKQPUdTlHUqfitCUeXvMzl4J%2FeNuWhcq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDPx7D75deUsDBkLicircA54spyqsbQEFMQXu5Q4h36KjDThu2edEmV0rMQ2oTFItpeSnKCZTAQsl24gWuqXPNYG1G13FtxDdtGjsTTN49IETabtBrxz%2FgJWoBjSVkQ9UhbLhoefjSnjU3YWzbpJGsn%2BR6Q%2Brx3aVdBVwBrfstJdL%2B1Ft01E3TX1e6f%2FW1YmN7zJz%2FljCUhmwc85%2BocJcUSPgX2ich3lAMbHek14HovCEa3oCGV50tTHCm4v33sU%2B8WzmxJkp%2Fq6TZVMC%2FZFfOvstCoomCZ8s8zH8m6W5HH4vGkULLRYa6p1hK3Kw%2BdWE5Xc61cKTTA0GYlOYBKZqKYiIjDqbGtPuY99%2FXOr1V6e9lQG9pp9rtNoa35qkix%2BExSZRZE3uy82wik53eG06URLSjnir80MIMwjdvwpvYXDKcN9FdP5rlhDrLGPROefhW%2BSs1wqR%2FRTTDVWZwNkPhzbx%2F0n83ATMJoD5MJqOjvD1fm%2FPDiohM9UFoFwqsAFEWH2ZjMWhAGritFHFNXn%2FB7tNCigovD7lhlgGhYjuhyLik0YXPO92FKz9V13CAlYnGV55AkO5UZSNxGPTdWeH7lGPVNN71vfX4NoFSVcqPRTdsyjKtKHr4VBJlyjOkXgKdJQyhgiydOzgj2UjML%2FQlsgGOqUBwRvi9M%2BehwF%2F4f%2FKHiYmz7TwHmFpvEk78M8To%2FRl2cZg1kW50v%2BIQS7anryQbWD4Kfv1A1qEq5fPhFEsMrIlRj5%2FZL3Xq2huB8G7yrxBJJ%2FOZvYMCZ%2FoYoVOlUUXCpWIV%2FKTuo0XvsX1%2Bzwu9OdpnRJGF%2FhcX7qMkWQqlIYQ%2FBukaVEtvBEny%2BLTgrUiqSjFRRvoAWLxXLMBtGQFWcb5QOSUzczu&X-Amz-Signature=b743df260f037d215efa1e7ce51eb009af849bfc015c08972661ce4c49f135bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

