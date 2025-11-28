---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFKE345C%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T120106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKQojJJR%2Fe9%2FUCJoukUItxPBhTP5iew19a0lH2B94Y8AiAZeWxdpKEAGQM0XRXqRgFZ6C0LUFOFVFA5WsoMKTwPqSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAf8m8tmG9BoT18euKtwDaDIX4rr8i2rRiTNk9QVTkU7ZSKuZmLe02oOPxcgOEeFmkosN3VhNg%2Fk4%2FpBJ1O3Xfi6ng7Wh023Q3zs3En%2FT3mbx4sOous6duYUykqdzNgYTEBjuRlTum2JZWajCyChZ8E%2F7mftsjf0%2BQuuGYGgUSneoMWKQQTsn3QNP8FvebPR1XDYh5%2BbTfzVWaBZBjNT41stb6BcRENnYrflKAvpwuMqbKOD9hKsQFYlR3eylLHiKIaZrBANyFS0Ng%2Byfo%2BvpuqzIk2tuo0YvXFjyeJ514R8ox63%2B7N0lsku7kh%2Bx2cclie7vJjLVxBBPnYRhK%2FoMkDRD5iuKLEBvN4VL7lMtBz5PDPF4uIBpBtuiZtN1QeIt2pBdsaKLDVRAoYNcAGOAjoxPAcKeOaHdsEafUTfxvn2taasulWlwMq9sZrxUvLrzw6ILLer6dkweyYS7GbT%2FPVn6x6y8znhomppCHkc0gJyneURqWj%2BpX6M5q2mzU7cwwNRnwbSmmChZR7FAC0gckiHHv5LdYGwnMuUFHyJFGRP93YYgXT9tlwFNFg8uFs6JfqTE1y%2BCpivJke6wdCDKywK5Jy2z9OJ4VML329rFyJ5gLCLLamlnHFG9ch59cvQEQEsVxBPpFJFAygowoNalyQY6pgG32B5NULyygB7n64vECyq8Km0gXH3t8S63YgJn2drYcr%2BNMAbdSMjz7s6Z9qnmH2XmdiutK3roBSZ8UdVSeVl1SzMBoEmQKCU4VOT8Wc05yQXMTVJv85QPSmRfoM6mt%2F82suPo%2BxSLzS%2B4uF2KKPcPnl9iAv70snETjPYziFVZTClAVXUmCUDE5o1NAd1HL3ea9PRgrCJtjgO%2B4dz7ZZXPksxTtjH4&X-Amz-Signature=d5821b630d21e8a019e0b927ff086b53577dd92175e88cae8c768362e7421b04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

