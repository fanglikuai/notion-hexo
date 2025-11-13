---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQXNUKRC%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAJejGCG0KSP85kTWEpu9o07bS5sdhUPRHSKw1sP7hjKAiB3qCJlYyFUq24ERa5Edi%2BWuUDFaL8Q3DlhNzYndbK5fSr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIM8KNUBrP8RboYVuo%2BKtwDplDZZxfEHI%2B%2FNzZjuEfgE1TF9QChy5pLzXRn3GB30hlCVfdD8zZba574Ts8Zys5xMp8ndm6UyBWGmaD%2FFtEjR8IAk92yH3tJwkaetZB3hyeREr7a2eW7x60t2cJtqgNtM1E8UFVbfYC1lnWzEpriq%2F2%2B10vlfdVSc5z7DOKA3bnUtXUrC7uu%2FDVXgCvbOnfUouPZKeZqMQ6yrJ58gtoyLuVkcAJ4RtWW4ecylhCuAzoHLZCYp%2BCgl3J3t2T35F%2FNt60VxWevpXuKsHwJkdyeuHm%2Bk%2BEnnvgpHpdg43vy44SLhgIGERQ8%2FWnaMrbvqX%2Fusn4nagNatsg3h6thx0rAHIwvVRjFQg8IDfQV8nlTyzuzs25JIX7OoeXlx6AhdqzctgIblaOMAFjNMYWvx1qYuvAKtMZU7NXZCzVldpyZCTmCNPV0KtsnOiVB0y4l6JEbJlHctLEAccwwtZMamhoz82UG1GCJB5Lk7GECEWM5zJt7T6LFZ9eeVTbdYnZ7fIqV4YRSl0IIVFlOuQUCoYHhQK0IycUrXdPNud9fBmx1NgpVOoeyk3%2B1ggctt0k6EVeNrbQIFcKgVJOXhH9T08Yxanq0EG7fB8h%2FuKDeegW1xyzODFqej9MJ3lhgTwQw7PDYyAY6pgGhtt%2BRcwjHT0JoYWFySzIvdijWSJ%2FCE%2FKkXk63kC8A7Ml9OQQhwX5JL%2BX%2B9UDdY7u4KVIndU5JI%2F3J0V%2F3LIMiW9ThcArhZ15UY5wo%2Bm2kQ7I%2BJCUosIS7S4Jo%2B6RMVKMdTHYzqMJj6br%2FF6RfWTNJDKrtESRp9p121nduEANsWgmkAD%2FR3jXDVKxUms3%2B8jrbeGTcXO0M3oDWVIVg26G2phq32rWq&X-Amz-Signature=27d9daff5831a664a049f524643f1c8f95fe2b2898f2a65c1a9e7c90ab2f53e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

