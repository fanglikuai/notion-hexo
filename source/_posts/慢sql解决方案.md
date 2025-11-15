---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6AYBSPN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUBk07B6BDX%2BFhzOs8N2atZ%2FYNa5u4RygsElTahL%2Fw%2BAiBHDIYEmhuQHr%2Fci2oAQLyzXCoar3pfY2XzHTxZ5btqWyr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM7wxbpCi%2F6ZXMXWqJKtwDuqhhRvdlfGRSTJ6TS3B0hQqt67IAjDEQ1X0R8RTtr7oFr7%2FBSWWVa%2BiEgVKkYCRz4mEqLADtNZVgK7F9rMOYvRm2fUvwPwTAajyBwwUQYEPWzXWeK7zGw83HdmA18Jn7Wa0Pv08tsbXDnHHiDo49BSgeMDtBJOzHPpSdO104WLE9voMnwrYihSpzpQxjegiop6bmEgRmMOlICUvXYLHAPl9x7JkzxKModOoxIo3PYReDSHVWNusH%2FWHZlOFJcmisZLNCpWFj1vj%2BMIW4L%2BKzklOTXbTdG0LC%2FR9QanBkzbBBXaxZ2tsZvwU6CTtJDBbOgh0Ve5H1NOB%2FxynmY9c9IeFRS2DgUNiahKCS9TZCfax25FT0ZkKkJ8WolD0KXylg9q9r4TiCYKaRWDDV5MdOjemFHvuOnsrtntqyXD24cmJ%2FOhi5Onpghnv7AND%2BDtMHOVRmhsSkhBdegUKC0mbdkwIUs3%2BGaYD5pM8dQj37QJWXkfdQqH63%2FZPB16eNTiGG3LX7F3Ggdy8283lXkhRSYUD6%2FB3n8kZ07HrGxNU5M%2BhbLxECe5kKpKJtpXgliT8VskAHzLcAbz%2F1sJbv8WQhm3Z9x5iuC2SY3ToQwTe7WX0WVZLma0rREk5F6kcwsL7gyAY6pgFbDX537sJrr881YWnGSosLz%2BOzx9kzu%2Flt5iLXC3ee73vrqMHDky3TXBHsjNmff79h5vKVbcjO8OqnWoY0WlevpV5HU9F%2F4hKikx3gGafwMF0aO81KJAh3vi7DDBCDt74Qqb0jzg%2B6iPyRVN8xwLx4JQ4Mb58a4Lo5ZhX4lCg8JO%2BOCYOS%2FtnF8RQ3y%2F8%2FdOO7dD3O%2FEsL1rrgyKK794ddTdFDVTFC&X-Amz-Signature=8ca3e220eb53cb3b3df21d8c5f473c55fbc8b6cd4c3fc0d7928868de90fbe507&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

