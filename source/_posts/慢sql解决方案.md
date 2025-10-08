---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Y663HCM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T030113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCIHP6m6y0zITWUGNm8qiYMse7Wo9sHALNVCaggT%2BJNibzAiBGYdQkT2BITs8iy3%2Boadxu3hsVmelruq8k81bGKDjXFyqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRGAHLnk8dYppnI1KtwDgnKXURAONeVdrvE7Sw7hS0wK3FSCms5s39megDizjW0n3dZ329aFVNeHmmKSAgMr0kp2blIxJeWRVKc%2Bln2MfaHTF7oxal2wbf2b8gZE%2FUw6Tt%2FZcxL4pSG2WR2md8L6e%2BS7cK9C33kMNTXa5tm%2B6xXft%2Bxl%2BLzxjGFuj9z%2FIedi4lBPgfq2iGH2LHn4%2Bl3PSCX8Fd2pC8dE%2FS5kCK0uJiaEXq6aR5zePESFnYixHvB2eA0Z56awQwQrI67Ap1NPraDJA9HmkJFi9U7S5i9Ec7TTjJ54kZjzLI4Vrky%2FGGM0uwyHg7I%2B6eVFsGWIH3rYMWczpMAm0gryFeFySOMzvXjh%2Frdaf3EkLDxY9wCfeQW4DcT8cio0dYEW2JZQ3ebNWTLqukhj9KU9u7bz6RBNC4Y6t7p0LWEICRwTQSjbCHowygq%2FVSU2UVs0XTJ2x5XSdjWsSAq3JqI6ARVGQQOJAlVO7YG5Ln8FLUaTVd3XHtGz4WEH4sEKrLajtOOYpiQW7iXL9xx7uyezqfnRmLH2JinYyHisT%2BRxNRVZ3co%2FFiNOSR4K7lZxsxJQESG4BevvQkAUfb%2Bj55Lz9uPiQPQ8bly8EnOUT60%2Blz3VEI%2FkAOpUa9adnkSPPqveDuAw74%2BXxwY6pgF%2B49GBjJhI6n%2B39h63XH11e9UBlMXoPBpHiYDL7faoU0CwaogV4%2FfdnNGhDViu%2FsXAucn1fenYpn5opTgpaHP1QlsJAurYXXKsfvlm%2FFYY5fsVOcDt7SBfMOXqXpd4XMyZ79r8ssfqbx5ErGlLC3BL8t26Lki%2FeoSk3fM1cAptXF5kmf3Pfg04QgSvs5RuarO1wLWUNFOMlygN4VC2rwNdVQlhopzo&X-Amz-Signature=275a06d67906341bdd43dbca5c46ecfb9ba6ce58251b5c745245cfdd8efba410&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

