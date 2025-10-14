---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXQBNHH%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6OTzKPCmXnMFxmv%2BgZtHpUgzzvVB2%2BX%2B3u%2BxQzsn5NwIhALTloXeoZQ2%2FboE6yCRuXO7b5VxvEqwr8SD3PgbXA9fhKv8DCGgQABoMNjM3NDIzMTgzODA1Igxxv7tYEeWknM1nkdkq3APHXrsemT4AUDCozNl46egCLS39zOLN5soS36dNkqpq2vfDDl2iam6EBZHXmmu4d2fSwDYTbWGnz9kITCOOkEBcHjL50Xsv8RmxvkQR1yT9Fygf%2Bj8PDRBG%2BqaJaUZuLAewHvCHb83T43oQEGhHcFUiCqh9n6xDIDcE1duWHsHDcL425MD6rriYwSydvBbXMRcVX9PvIEnOX691LsRTNxz07uK68CJQt5L3R5gN84gCEekrIe1gVt8wYvUk%2BZkCIQsAA9JmNrCx7%2FfaXJff4LURDYVyTktY%2BMGcIjaoAsK%2BC2XwrJ1wOF1V95z4vAZ%2BnA%2BLAC7ppfwWVLqfEAWEESrwHLZb0xwuLa42hhn9uzpIttCxVG71%2BWJZ3GbKNfjzYELleVEQa84dsLsc1KGlbS4tytbSaeY0zou9RrVS5zdLqGOqhYUSnnVCkVECnuUWlYQR3ygmNhWHvWaSy4UvedTeACgyK1OA5g1H43KD4I1su6jpQTZcl8QRbi2agAHR8KpKZsxS0IgBS2DZqKZfAH4rhPVFUB%2FJkmBq%2BHHML3%2B7udDznWUxOsRR%2FeEhXn9ZN6V2r%2FsfT5rWCeGEIg744ZxcVnwUxhdNL5%2BBkDJLevHPZNKbxFI2xc1XTxMkEzCkprvHBjqkAYBOCKDIO8JFyqvvpQsGE9qSM%2FBaOvBXNaWIi3IGUM3aP1KWNjxHwQ4gQ%2FEIp76QY2i%2FEF7VGE5nnAbidQHE0uBGW3A2l8xKe3REwcXm56szDsDm630xVQolGJh%2Fe7L2FJs00N1MXjI9upoX2ZhwnnxhQSL9YB%2BNhu%2FS8I%2FN4IncqTcH%2BynhnZsFbzdX9HEkoyKxTy4i1Xg9%2FAyMVgloA4Wcd6wO&X-Amz-Signature=c2fd6cc66804f171a16b58dc3ce7186f3ff309141781d953dc42a38667e604ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

