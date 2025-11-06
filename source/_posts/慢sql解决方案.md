---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFZJJXD2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFWXza5ndkioXJ4DG7l2v5XSB4Tr%2FAzQDPWzkZPgi1cQIhANnEPKCANEv0UtJ6fbM4t4StOtUrZkM1wlKfD99BWy1DKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx5igE7wqLrr8I4JMUq3AOFGRpXua%2Bn%2FqzwSLjmBujwAEix%2FWC3MAeBhUNOHw5n1riiVzqGlhxoRNIn7nqfCaNDkylzlz9KC8h7CRFk%2F3%2Fuqzh7yM9A%2FQ1%2BbjYrftIoduYeV%2FYsWQJZW5e09Id%2BEMN6Nf0%2F7gieICKEdFWOYJkzry0oXkYRgzVgDmhkEEDuNNcFDAeAYZ0OT1wcTb%2BK%2FQeOiHp3vMVs9KfWVqXCdaD2iwfHfcosMMhSmwxJ%2F2aag5Z1isVTxxNI2StlEN8HxUzAMbtrFNsLNb5N8CXKadyI%2Fjb%2BinsEyZJkItuB0C6uSekyzI56C3KrZuK0eHGnNWiDsVWBE%2BVj%2FmWVU6%2BR96IX%2Fk8pIuEA7QtG8hFHAr1pdUJhWgiASTHJ19vcHT3LQtjFtQLy3n40n2zVn6DPyhUOCAfd4W6wq67ty%2Fr7aF9mxD8WzY8Xmd4vUAmYnJG6%2FyHP8PvlMh0F4fQnKXjwPf6A%2BN7FO%2BIfZW8iHHP1V5sFe2cOv8EzeCSqatTLTqK0oFbf9pED%2Fi64L6SV4KeD4qZL3wdqNmgF3a6%2B4mbSxrgDnhkvYgfMEQwX5PWYMrzJZjJ%2FdNfURRNGNOlVrsXiHlMqAbtjTkOIm1YQmF5sEJhdx26d4u8OPgturlUgADCTwrHIBjqkAe0M8BKxJElJ%2BFkhT8DNcQPip1KlDLSy41ru%2Fnc1yb2fOZEZKtKtOYxOZRq9zA8OEBhBqpO1yWqVh79SeAwBwzTFAfIjxpLbsVALiQ1%2F4881g3gzzrQK5zjykQt7XCKUxSrrbdMkhF5h0%2FztROA5UeUmw%2B9JlX6nwX80DiGyMuWrWW8xDJxt9Dl%2B%2Bn0Dc3K4idK2Q27XqFKN5dSGcA8Xo6N%2BMMej&X-Amz-Signature=b7491be51af7a09fbc324ef2fe7c940851f3f9c9f69188e09383c24002cbf349&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

