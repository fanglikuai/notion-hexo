---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZYHZNA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T200051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCCRO%2BZ0rT46nhxDz6slY0jbnlku27tFB0DnaoFbzfWQgIgDF4OGfO75VS%2Bs5y0kEMcvtWVkbVsS4Z4hdCksayM2nEqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHwapwefq2kg737iYCrcA9Lx6KJRsJdelgcqNXc5pJs42uwkd%2FDtmvkSrl0CQmex%2FmtM6N3QDQvI6RxqR%2BzC6A6ooTqY506ApK7rX1JocI2RblQSia3W2bc6%2FE9x5PnWGORIHTaVOz9e1zfkPTQI3ThXzri85IdpAYLkZmH5%2BBgO6IZJSWZmiTPUyv4L4ToPseBAjbuGN1C9TlZHcziKhB5OQ1THr9m5EkERWzDKhFEmLLObU5FUTARo%2BdIJlOwede5QXCslla7cdcv3DHZ4M%2FiksN5EVxiuUTHhQaON4uQ7XFHIKoxjnUMgzfbMkM3idZowOu7F7n3My%2B7xOsrhqcog6FyLQ1F4QFGZt6sp%2Fy9HvNjOdXdXU32h6tfmFa031pMk4pkhaIIq63Jh%2FDb7tPLxonVPammlJVCY5d%2BeN%2B8nIHqk9qOK%2FnQzNfC9V6O28XBxVhYtmcKex4ZjFZHm4AlaBx4mZwm6BbmGb8%2FqS9TMNDhvRQ29rRNHZ%2BQVmY%2Bybom4IwpZOp%2FYyFKYMPOOiPv10df87HpRmJrX7LB80GfRFEqYETqQORxxRC94pkqFAwQ1gGWuaZlEuNY%2BLjRqq%2BMs4pfNKlwNwL0kn2GHsx8DrQuft6XLaTULdDotT%2Ft%2BYc3B78urDclmo%2FWqMPC5hMgGOqUBGCH7Tcq27RLL2LSUiWSCkU5Dbfxvwn3h4YRd4wJab%2BWDtOqaE%2Bi1PpWf9%2FKYNLH9RyY8cy0N3HI3erEqjARoWcml%2FZpRlnVyzjWqZtr7BYJ%2FcGqrc4nAHMLwhg1iePwPn1McUyZPSDSywIoK4zNe5DgChj8iwCuWhFUtbPzXtwIv9M25ejkojZhML6yjrJAeW8SW%2F1GVk7WeXK1miC3RlOyjxMe1&X-Amz-Signature=1e26576c7cc01f877bfb4bdd68b2b15939227974d69cafcbbce9fae966f79d26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

