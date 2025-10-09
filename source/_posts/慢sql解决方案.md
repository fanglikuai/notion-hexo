---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676VUDVUX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJGMEQCIGXU10SPV1F4q9eunTe1cqyRohWLF2TpJ9SYEwngnP58AiB83C2Q4FUOe%2Fs6EIWMoc7VyYIBV7fxneONbkND5G8LniqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsfVSbNR8T6kaykOuKtwD6PQ%2BhX37pNHfjRYp2vX43kRliy7ToPLjTgqe5wWzxpngt5ILm7ONIOBcJxJ63yWa8W84UC8LezJTvYhkBi3UntM7zVeLje4F9XjKTUz8tBV7yNDSGf%2FJooiP53wA2lDmfpikOH8X4j4Po1HOyljWcEyauZXroc4RwD%2BFX1q5NBhAWOQ936SkjZ%2BpzQSYNj3K0iZWHxj8sqkS%2Bbv4gdHM1OymMGPM8cpItgzVOBa7G265cJQYC2JSobfO7N4%2B4CKlmNpfNf3r120GlxstIvhXBZOpLWkCTFoX%2BKZcZ%2B8Ji8h87RzPdlY0683Y1UFeRGd5g%2FWbQcJR9i44td6wV9c4aKdA7Bi9W7o%2FBXyzaTZaU27T3ctSHfil2HmFhPBonQUu1qKbF7Qv%2FUrMM1yH8kGdqAcKwtCm%2B6JVA%2FZmKgtMfCulnUVpC0G%2FrwWjqT%2BOdvDCjV%2F4%2B9mfKhQnMWaYD%2BGauuQiD%2FuvD6ejNkyA%2FQFHR2Dp1TeZ8zHGna2fZQq%2BgxoaFcWV8LA1AosMtMIYfeFp7%2B2uV7dYIdJzAY9cj%2FsIuUIjk5ArcIPg9ww2Jxo6sIGyzHF8flKD1uUiJEm4%2BMZD0L0Z0o%2Fs8HzaLyJu7uT445yAWDm2csHHfjGaVqgw6sGexwY6pgHymwf%2FT5Ijd4w%2F7F1sTHikaxCaU0uYC4Q4lubANe625ZvTqf0WTdQ5gTMepMq3FiV2wHaprp4KwssOXQyza%2FcmLRQcRtsrAP%2Fb6iKYfJfdk9uY1Y%2Bf%2FGNlV%2FAeC40i6X222KxmnQIk6Z4NYVvdQf1F0Zh5YojdIs4OXuY87b5Iw75HblX69c0wwveLMngPQcov0T%2FCxLNAZDYkEZ2cqEEyi7pDdng0&X-Amz-Signature=7f2769ab90ab2dacac7b754543b326d1524e8abf358a684c13a06e455f303512&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

