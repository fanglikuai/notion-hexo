---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTRAEBY%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCc5cXmVBhUrU7gNl2YmZU5AoXOqvAdyCYXnQ%2FLegTKZgIhAN434K7g%2FekByjzCjVdy6jPyKHq9UzgZ%2FLPf01y7I%2FwqKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxrSFpbEWTyTOMRpeMq3ANY8yUQFC00EfvdEnFKOlwl%2BJrhGnrMT2jVQenF5h%2BbOqY3t5PJta%2BvZjcjJ0quHCcbXewWvshh06005KC%2BTwdb18kRDgVHdssdfzBJqkS%2BZ6UhVIbyCy7x2Xttm5NcfqGuqRgk5teMpSJ%2B2GOiSt2KBf4ZdzsILQHcu3tj1Vl24bOYZzuZ86l9N0TraadWP9vrDwqsC7AdQ8yT%2BVlo7lwxqEmIacHSTsrfJMU0v%2BBRZkzGdE5CvkTpnLHz3ZbzmV8AjsqkT1Qn9BukiPwFoefeaRPKX6UeNee43cHGg%2BBkQE%2FRWkoeRC%2BaKoz4RipvRvK8Zz48X63zop%2BUH%2BziODl1YHJhztV6OztTN7H2kl9Tr0N9swZzBy4%2FHc%2FWnCF4OBJhD9Yd6clMtUDl9k%2BGeQ1%2BjlsP1xJLCr%2BuSypQa%2FLe8ZAE9ojAxrNRyODpNm2NHrsx7VYsx2PcMHiUPorXtKc9cMOZuT9sgWgsdLLnasez%2BjvsKMjSskCdQDKR4T5BNzFYvFEQLWE9Gz61sCxcbgN9EvqY94QJy64d7Ev4Y13eQLhEAM6C6r%2BDZwW2z01IMIxoDJhjVF2%2BUTyIIO8QLaeXG%2BHF1DljkdyWb6JrhI7T3bZZS8lUbxweTurS5TCk9%2FnIBjqkAbO0kHbullT0GxOJ6HNkl9owldm8XPHnGG4IGqQWAQSLpx8zm%2FXXKus9h3CLCJjLXLVOG51DcxMeLejzbaAUs7GJm5tky97tMn5aKR8V7ulz6E%2FDOhGtjqkB7QwkkVz58IGGSzlrZKyXJB4VJoARz05jznDE3zcAP1OLDDH9m8RiTOYR6oeTTGPc%2F%2FP4NW3%2F8Pe95%2BUyTw5cS1vOgkbocS8k%2FPgK&X-Amz-Signature=f29ffe751e14f4a41b05a178754c78e303eac6165c61f42a6ad83448d1a36868&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

