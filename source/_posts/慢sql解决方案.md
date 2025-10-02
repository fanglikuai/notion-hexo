---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBFXAS4%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8PY79fvP4NbZpTsG8belQUaqJqqh9ORKcPGI7GJq2QgIhANQCBwrDf68OnZ%2B%2B1D2j9i60q38k47XOhLLfInjPUp4cKv8DCCsQABoMNjM3NDIzMTgzODA1IgyfhYKJfZld56tCif4q3AO8rWk%2FLSLM9UWSKBMuLK9xvzRElAWv87OfKXzgMmbjGf6Llh2Q%2F1Q%2FIuxDAkYefVLz5CK3jF0sUhNZWtsL8YHp3g%2F432W%2BN%2BjNESezz1vJ6%2BHv0YC6%2FwizDaWz2HMtJY5qwCCfNAzgwsxoaPaFhqZ6uuaqb6KCNbSEqAjvxBRDypM6lStS3rZZhdiq75gerowUV6cJJ5P2cr%2BjCpqhO2XNmqr1Rioz91gzFL1dhqhdKCtyoMCqMvEJV2L1D%2BvcEAFR9uesdDASg76wunnAZoJmDMUO%2BqQcSMWhNcIXhr%2BCIsetSgX3CpJwD4M1aIH7jKl91SPSmSJBfQQbhB8LIwDifRqzg7gdiC4iVUnpkw1GYZCbXOvXWty%2FhGXFt14gumDI82KAw8oKQyG7O3niEE%2BNPVzufe6B%2Bd20RjegtOPy1UV4BHjgfNUZ5%2B73cPZ%2BLeA8bHq15oEJsL9UZUwZBNsK1Z6tslFWCfgNqKLvDBMZ9j9WqaIs2M91Ajk5RhpLCgZLXeJZ4r%2BmNazJ1ilOV1p8zKNfrooy8dxTr1NBPxS%2BFSXXvym1HE3syI7FiChf3lGZAyMkJ7ugOutg%2BHUddUUzwAMegKE1yTpX5hMzTK4ekjXC%2FEBf2ajGnCP6bjDHkPnGBjqkAZbHJ7QrYGhY5zXI9iztCBo9KqHmvPiZQDE4QiDjh4YL4cki85gf31a32cU1yilyg2a4CAU3BkNQLsAZ%2Bh9f0prpAbs3rqDQDwUWlB1YYMBLvBspujK8coVBp7S6ZhcE3WjVg5XoKPKiKfzZtwT9phy0pG%2BIC5pcwr9I%2F%2FpY9Kz6CSco%2Bb0Hh7AmMnA7kaH0kD382O2pM4RyvxCWymxhWxpyOkqS&X-Amz-Signature=d2290e014162ff98aa0ef84d494faecf682a597b00ffd0f164d33b274c2d99ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

