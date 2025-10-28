---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZYHZNA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T200051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCCRO%2BZ0rT46nhxDz6slY0jbnlku27tFB0DnaoFbzfWQgIgDF4OGfO75VS%2Bs5y0kEMcvtWVkbVsS4Z4hdCksayM2nEqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHwapwefq2kg737iYCrcA9Lx6KJRsJdelgcqNXc5pJs42uwkd%2FDtmvkSrl0CQmex%2FmtM6N3QDQvI6RxqR%2BzC6A6ooTqY506ApK7rX1JocI2RblQSia3W2bc6%2FE9x5PnWGORIHTaVOz9e1zfkPTQI3ThXzri85IdpAYLkZmH5%2BBgO6IZJSWZmiTPUyv4L4ToPseBAjbuGN1C9TlZHcziKhB5OQ1THr9m5EkERWzDKhFEmLLObU5FUTARo%2BdIJlOwede5QXCslla7cdcv3DHZ4M%2FiksN5EVxiuUTHhQaON4uQ7XFHIKoxjnUMgzfbMkM3idZowOu7F7n3My%2B7xOsrhqcog6FyLQ1F4QFGZt6sp%2Fy9HvNjOdXdXU32h6tfmFa031pMk4pkhaIIq63Jh%2FDb7tPLxonVPammlJVCY5d%2BeN%2B8nIHqk9qOK%2FnQzNfC9V6O28XBxVhYtmcKex4ZjFZHm4AlaBx4mZwm6BbmGb8%2FqS9TMNDhvRQ29rRNHZ%2BQVmY%2Bybom4IwpZOp%2FYyFKYMPOOiPv10df87HpRmJrX7LB80GfRFEqYETqQORxxRC94pkqFAwQ1gGWuaZlEuNY%2BLjRqq%2BMs4pfNKlwNwL0kn2GHsx8DrQuft6XLaTULdDotT%2Ft%2BYc3B78urDclmo%2FWqMPC5hMgGOqUBGCH7Tcq27RLL2LSUiWSCkU5Dbfxvwn3h4YRd4wJab%2BWDtOqaE%2Bi1PpWf9%2FKYNLH9RyY8cy0N3HI3erEqjARoWcml%2FZpRlnVyzjWqZtr7BYJ%2FcGqrc4nAHMLwhg1iePwPn1McUyZPSDSywIoK4zNe5DgChj8iwCuWhFUtbPzXtwIv9M25ejkojZhML6yjrJAeW8SW%2F1GVk7WeXK1miC3RlOyjxMe1&X-Amz-Signature=cb05da26ef193e7d445318a0d50af2360a3188a07931097a5c55c20ce6c75d25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

