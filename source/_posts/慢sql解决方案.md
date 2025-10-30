---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5UTWZDI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQDPPvryBc3EKriKTBIJxVX4CzHXnG0eNBzwK2bFT6ZTDgIhALZ49kDWVVubzUl1aRckwKhahnwef5EQppj3ZBrRKxQ3KogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyuUr25SzAj%2FKsq%2BV4q3AMEHaxt6C26VkGQOg%2BmgKcRv%2FFE5LoSHU1MgaH0rqyqdcDjwKZit6nCnLGuH223Jz4XUTFRlYq5c2ER2Qao%2BXNHQhr09FFmzs1EjL19NQ6GAEc68URFUAPLbyvwZriPtkWMfKpF1rDpMKqtMtY3sRFO8%2BzWxTVk7TcT85OA4pEbBkBaCifWs3srWjvc8%2F3CYNJz46tq%2B96gHfF052KboU4t4KvNn%2FCG1zx0jtQO78x5Tgle4n4nDovzkKkVIjrKW3MzyLADfKZhfnuBFKDjK2hUKhHzjkqlrqKdqgEYbkdA3YhGJIDrYbSrFcbcgWS4P73o%2BsCLxuU9H46rxhvdZxfS815Knv33wqb0%2BylwY1FQlAvjWkubB58cu%2F5p6Nv%2BqMLX5nwX%2FWtrioMpbYBc9rOh%2FH6u4axD25suQ7qFUTIZYxPNuRfTId00tUXZ55MQ2nq2pwm11s0aluxf%2B%2Ffcm38tSAUsfbQFCaA7Cc6QNxsGeTsRxOb2j5T0XGAywb8lRoDllIU28%2BYW9Y5QQS4SK9DU5u6Z78f0J8U4vRn1pNhTVJ0WtadZiFjkLRmi47IBo1MD%2F2ZRd366zRXB8D%2B0lmILAaqI%2FBB7qWlL3k%2FeiJ23l2CmF%2FMgvHpMTFANKzDf2Y3IBjqkAdOIZorkZdte2CzK6iTDr75yEFnVKi%2Bcd6g1lWm3Xtrf7bY1KnCypHJj9V6KgDrmYo7UhziVfdA9c0nK%2BXfwVToGTk2uVIPlnVXtINgEkjYcHyIhSZpWBRowqA9cT2XjroApnoPixFngyz7o0bUcB52UU17s2t8sSt%2BXUTBVVI2YIoRZc2eHdD%2FSxNVl2x%2Bq3Zv1PZrBuL6ZbsKAzmMhO3svO9xw&X-Amz-Signature=f25a298a4fb7c9e4c5aa344301b36380cdf4bea0e2cfa65d8acfaf0ef3fc534a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

