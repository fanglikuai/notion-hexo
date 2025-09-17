---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R4QFUCH%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIGh93wd4EAyu2NZ%2FuzSAsqOBXiLeQVn4NQrVwDmBIHYwAiEAvEkSbIXHbRAESOwPNAGoIyXBqaawXeG2hUhxp20dgiQqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIXpRCG1Xz62nrPdRircA2esSEc3zhq%2F%2BxJpBZU8CQYl3FNFC%2BaXN0Rrwf6WuAJ2qvDLVGUZoKIsjWv5%2FaTle%2B%2FcDBqbCMc%2FThKQ%2BdReMJrss2wpGLI7it%2FmQKU31zQ9Gso3G%2FBYA4CmrWJcR0GjfbuNSJASrzK81IgadyaTtblkhVXTAhFmFf2jLBhKA0wTebHPl9yxf8xwKhHXMi%2FelSsl2UU5%2BmwwPw1%2FeQNWEnqSp1G6FWdVcSujjkIFytsLDrfOkTLrEqq%2BuQn8ahuRSdxP9JQsLleB0rhiwOLsAE2Kxlj0L6OXtfB9I9u698fL02fYPqlEat28noV%2BSeaRIeuCKoXKz%2BIskUl6NWMcDgorXJQgOzCcQFirFNdSseV6jNSt1p%2BE%2B5eU2Jm34111vpl2h%2BniNAYZ1cR3%2F4HA5LwfQ8u9wcUsCMUYQNECKMli%2BHGDxabrFKPPL6OMq%2BNsgBpGeWqaBJO9UrkYyxhY3oHYpi8eRZJxxKx7tsabYA9rzg9I8mKBVlL4FPP7TU6RIkTNEVWzuaMPs2ezKWg4JvBYNdgJngW7yDyiV4LF%2BjyJOdJV4LbgV9XAkYUL4Z9RA6BRELvGCnE%2FyJF7lKp93H9upWUAmSNfHZcuAMrnBcHhBqoVsgE74VkYrPu0MJnZq8YGOqUBfMvhRww0LLsLfWGocs1f64j6jG26g0j1vTriZr7cm8TdIztZzsAepy%2F8ZC0ClHkKDkdA%2Bu3hUqZW3iDk0OSXfd%2BjEBsOlOHY%2Bg%2B2RRPsa9ZB%2B3rcJPO0%2FjIQRuRNyxo7tMqEVvkcvxDWZBV97z8dLueRUkC3t507iA4UjONJyPv9ofP0pa5JBgVCgDfCXWnVT7JfYMQelu2MqUKtpEY7WLpWAF8y&X-Amz-Signature=364faf7a10e01c6cd34f63893752a549180f4394de84d4c43e9a15604e73c8af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

