---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663AG7JXZQ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDcvkWUjWd71vMqs79xzvHaHaAlzJSkbftdQmzHFqbqKAIhALBWb%2BZAVFH9YD6%2FsWVAHR8ZXjZK0MBxB5NhSS4fGARuKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhM4nfrm2EsnVPbVgq3AOSHK2MeEuA1QvNwUXS0m2IXhO%2FWlBR%2BNyunWo%2BAduNxd2shLCYOQwnN6Ize6DfxP3bT4A%2FOQFu4dUVhiDQYw0XvblfA2lKBSX5dBZT0N%2BQFeASkiMJyZt7q%2BjvgMOTHJqUn7qjsQIayHXe2EESM%2B%2F0%2BbFZKxmVU%2FyXr4xk7KuVF%2B1Dqx%2FhZhm33vdxdNSEr783483%2FVgcN3q%2BnaOtvISiKxnZkDpw7UqU0JyREFQGikn5eg2BOLuHM2Ae1WAmXW28GUOI0MBAOy7tGX9kZ0dmJXPb4%2Bgg6rrpLtA9AsNJ9hJUUHZS8uomXv2JyY6HNyq3Wrt32sjZNmf1TRrrLxg84Uj7PvcFJl%2BlYqszrEHB7JfncICF%2FfdKWrKo3ItvPKNPru4mykxU3kbhLdn8ScoX4FOpEKNtUpxW%2BFXbTVd0%2FO39ANtvzWBlAOgPkjzn7VcwEV26B4AlTBukY1l%2Fc33ayBF6wo70kUqpD2ed4pcAUY0iJ6R0%2BBnmxBCBzNEo%2FYLQUYtnUplQSfdWTGzWKRAXTR%2FYdDqjFwmk5Aiy8lbaNAgjTLLdybzff1G78NqPPi0FyOYxgnO5ki7cSGEAN9dtHEc2oaAxwpuXmCA6L93iqNZMpR5Cy80vEKdgomzDasIHIBjqkAX0gAfVsbmvvvyhOK2r%2F69hmyT4xkxGdv71I2TFRHlaFXF6M8m%2Fesd2RbNiN%2F0K21VefsJL%2BdM4y9M1gtrlmr2CJurw1wcbEbnXURzJfOcipQ%2FYg%2F01ryx3VhQNQHTxWDZW6jChlsvHyTnzLRSV%2FreR4GCaUNnvjKOEnm5nEgN6SjrwI%2FYVOtW9KB7u9bsQPZBksUexh77sEO1hV1t499lPmO9Zt&X-Amz-Signature=3e08192aa01cd23099cc0ca89b4183d749b8f50266dc2002ff591978bb306539&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

