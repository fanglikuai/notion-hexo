---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TTLF4CJO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBQqfDnFKkxbaAAEa%2F9gqn8qNn2zSc785WoCNayzGLJKAiB2iDnL%2F88y7aOSSEZL6DKrx65j%2Bvgt8fAHvWBw5i0pxir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMp1la1xE4HOuETwNhKtwDprSmcqPNsxhRjyT%2BhCNjc39ZGZAlayYsSOYEuJTqPuFR1pZbqa8tnovzH5IPzUsWKN%2BhkUQFJFunh62nQrT0jUH%2FN%2FyET1dmVscAfEqhJ9VGyrnoR%2FH0GQvqR%2B3DD%2FGzO0oBJI6ZwxiDQtXMTwqpFNcNLw0sXIRvM5i1zkaId%2FOwVxMdtQQi5Um0mkVTnfIt62qMuKZZUMEg%2FGopjfax9cBRo%2FDj%2FTqGzvTQrr2tW5WSjKuKsp1J67TQWOdSYqYYGFEcuqK2pImq30%2BRJV0IRnDsTPW%2FPKnydCTuUJBCXDdU7gSFrLoSJ%2BRfJeieMI8CEvkYfox227uGdyXmyZ8TdWGOMeMsHdMFBOKNIQeTrcdI%2FtR0vZXP03Xn7aWTDAj%2B0SqhIe%2F6ibsVQrttqnDviJ8mhznWwKQXkKs%2FT8QXhDzHu11fV3utjBe0fIxGOFAWy6IJlPwp%2F6crDngIUdFj1dNVle6FROaTD3T%2BGGUXvxHdYMnE78OgZxSFvPb9acj6K7wtiu8ElH0ax7%2FtXTjMGH2XJhpt8Lyt1JG3kLd6b%2BPLgPoRUMBmnKchSqPAtlu8QCCGO1FMN%2Bt0LqupMHVt4GxveavdfazERm%2FzU2JlhSd6ZG%2BIWZAjcfsYSsgwvOjAxgY6pgFLiQA%2FXl%2BWZrwASni8ppc2I2JXDau9iD3ejiAViZPcl8PVazl%2FexAqXDh%2ByRCD3h1MsSx0Kn4NT90VDBXH8%2BV5zXD5rCkPF45b8J80OEN%2B1%2FzjMPE9ZnJfuKQbZPA7CoyxnuFMVaJVEB00tt%2BlEQw%2BMweX%2BBfuilbZVWtMezshbMMo29OzNO9jAjXKSzdPnxQGz5NVmQ%2BaT44CbDMUigZJ3HFRE2X7&X-Amz-Signature=272e77ef96f4b4cceda57a76dfae78270b5924fd433d79daa30a152f5a628c19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

