---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVGDGEWD%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T020148Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCICWOqKNj2tvf8tkrn8RpP3oxyqGEddsbdFqCjkgMTEgBAiA%2FPVw%2BFCX9ReERd88ACppK7lUI4slhvYOmDpmc2n9muyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1orxCkR8XnFoUmusKtwD7wI05W1b36lSuoIwlz3yD9vICVXsmtZFzSe0wPH4aQGg2bUnKwvCOKi%2Fl61z979%2FFak250X0ea1Njvml7TgCDhsmNZq4jSQnKchOU88K0icAem%2Fs7kZbIYkpdHJWFhLRdLtOUU5Qulc6SLY%2B3fl3KjG02zuTDwn2khRgH1KajiCTr%2BK4Q3uwe4%2FSutHn8VQcaI37UjVk4UXrCG9mrGKxDUue0hqK3LvfxwFSTxnBORwjHe%2B%2BZ%2BJpI3I7%2FrOJI1fmEcWAorlWl8dVygGJ6nlmdIxoNUY576LN0BeItC0o1stgdNOyO23XUFoYaYaHF%2BO85NnROrpzIMlUgM7%2FRd2qFdqnJX0uEhfWo%2BxLW32RBx%2Bwn%2F6rheGmgVP4MGLDX1jC3ysPapwUFrx3J8uSgCyP5fNkbkdxfP5kSQSBUqH0nM1wYQ%2BvgpO%2Fp3i8wq4YzvyN8PgWizQvtZ51sin%2FJ8b1y3c%2FslGRvHQp7WGGDI1g2LNp7UuoKIMKSpMYD19tlmadewQ5NvsEr%2FjyOYyHa%2FbwB7nmLW%2BLJTi5zXxV%2BTywDvyLgj%2BM3VDAF3c3xLdRvALlNWhNEshDUMQki3wJRFuR%2FdJH6N0IFyMgmAv%2F8998lb7CwyxUH7de7HTe0vIw8qSyxgY6pgG%2FcP4C5geOdrNMDQZaz7%2ByXrjXDe7purF%2FCar3kN00JfOrhIGK%2FDzfLEQL2nkmTwNYwKr9vBoCQXFfQneNkEW6FhSqqh7qYHmXd4PXGmuTT8MSjg3COcQ9FSHANirsfHRMV3kHQQJp%2FA0AREJ%2Fq9sviAcMjRFY40G558%2ByC5W7ekRcHlqIKjg4wHm%2BQfTWv9ZXA%2Ft4vqlkQBkXW%2F8PZcgUcx%2F6vjQz&X-Amz-Signature=29afacba86e365fd9a8e72a29dd2169f57bb66e88993679beb51c60fd312b135&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

