---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676VUDVUX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJGMEQCIGXU10SPV1F4q9eunTe1cqyRohWLF2TpJ9SYEwngnP58AiB83C2Q4FUOe%2Fs6EIWMoc7VyYIBV7fxneONbkND5G8LniqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsfVSbNR8T6kaykOuKtwD6PQ%2BhX37pNHfjRYp2vX43kRliy7ToPLjTgqe5wWzxpngt5ILm7ONIOBcJxJ63yWa8W84UC8LezJTvYhkBi3UntM7zVeLje4F9XjKTUz8tBV7yNDSGf%2FJooiP53wA2lDmfpikOH8X4j4Po1HOyljWcEyauZXroc4RwD%2BFX1q5NBhAWOQ936SkjZ%2BpzQSYNj3K0iZWHxj8sqkS%2Bbv4gdHM1OymMGPM8cpItgzVOBa7G265cJQYC2JSobfO7N4%2B4CKlmNpfNf3r120GlxstIvhXBZOpLWkCTFoX%2BKZcZ%2B8Ji8h87RzPdlY0683Y1UFeRGd5g%2FWbQcJR9i44td6wV9c4aKdA7Bi9W7o%2FBXyzaTZaU27T3ctSHfil2HmFhPBonQUu1qKbF7Qv%2FUrMM1yH8kGdqAcKwtCm%2B6JVA%2FZmKgtMfCulnUVpC0G%2FrwWjqT%2BOdvDCjV%2F4%2B9mfKhQnMWaYD%2BGauuQiD%2FuvD6ejNkyA%2FQFHR2Dp1TeZ8zHGna2fZQq%2BgxoaFcWV8LA1AosMtMIYfeFp7%2B2uV7dYIdJzAY9cj%2FsIuUIjk5ArcIPg9ww2Jxo6sIGyzHF8flKD1uUiJEm4%2BMZD0L0Z0o%2Fs8HzaLyJu7uT445yAWDm2csHHfjGaVqgw6sGexwY6pgHymwf%2FT5Ijd4w%2F7F1sTHikaxCaU0uYC4Q4lubANe625ZvTqf0WTdQ5gTMepMq3FiV2wHaprp4KwssOXQyza%2FcmLRQcRtsrAP%2Fb6iKYfJfdk9uY1Y%2Bf%2FGNlV%2FAeC40i6X222KxmnQIk6Z4NYVvdQf1F0Zh5YojdIs4OXuY87b5Iw75HblX69c0wwveLMngPQcov0T%2FCxLNAZDYkEZ2cqEEyi7pDdng0&X-Amz-Signature=7888210abd71013bbdc934532d076c3ac665a1f505ec3bcb42898d5c2b839df6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

