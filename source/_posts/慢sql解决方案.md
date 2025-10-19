---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHASA4HN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIF69D%2BYfvbp6DplB9Q6JyiQNpvjA3YEL2DkYiJpasOO1AiBXfH%2Fk1ZAW%2BGCwqzqOAyPW%2F0FPZhp2UOPvw2SpfodrOSqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeR4YkxBk7TLmr2nAKtwDO%2BU4an353OPD2uzZEjQBWgBeMppiVw9sPDMKKcR2RUuSCQD5Iyk1gjeSbibdhU9mY%2FkokWBdl3tmEur0BxkQaJREDfVWSsw8uc1ez%2F8BchMOy5ZLG%2BC2LdcWqresSQJ6OIvxPHfmg2KNeqDXtCbgYdi1XPVHtkjowZaRYwpY98jUD%2BncxBWN5eT13td%2BmVlEMm0kMPMNNze%2B%2BW8pX8BXmvU7Xn4uQlhOsZxt4wae7lB4jIRWBFkwG7M8tFYXWQeG%2FkTPRd%2Bj%2F5AkpoC7a%2BpcGAsTry6uYjR1CAsi8NJ5R88DZVZq%2FkWzW67YAXOolaX6vL3FPR3Q8VeNhsVCM%2Fi7fceABGjQGMKX6RKZ6DSmAC868%2F%2FaJkYWLyf%2Fi%2BGb%2BAVmyPICj4zqUVmu%2BF0Lp4%2FkU1hywJpvdnS9BOiR%2BpZ9v7OXC%2B4KHgJAKtUzf4GD0cCO1clwtvMrC0fXoveI5lFeLCNX12c2U3edkA5BvUQ8p%2F9eUJyNqqaIl5ck98Go9B8Uzr1VvVns1CpuBFfSKB3D8PO5PeTlNEUvifaK2fsXkEsaZ9rVW6ATpWTGNIQ7aLxpEINvSyLc7kWzVt0V0OmysT6zBd7%2FQA1leX6%2BqU3cFSiR7WnNq1PuV08eJ3Uw%2BZjUxwY6pgH0KCPx5u3qNCQEkoypay00jhESdjjfFlfzMcCLLMMeqnbawhUrGd9XFDsjz%2Bvs5QmcEPbavz40BqolLvwyL34thhfWUqC%2F0cUKDKC1PwdoXHwdBVomeqAMLn3%2Fzb%2F1ID99Jp4sXbjnRm%2Be0K9N5jSSGve%2FfwmjmksOCodlr0tupRaXyWpn%2Fb9bqd8j64vwRn1h2Gp15PgApe9%2BF9UwQQvIs0ciOghm&X-Amz-Signature=04ec6723b1723fef1051ec449727af8689700d22922201e52e61b1b3123d1456&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

