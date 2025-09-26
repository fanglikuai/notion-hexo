---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXNIA4TK%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCZQCVHVLOxubSEWSdr62e7%2BvydJWfE5oX44n5qZI4DlgIhAKrwBF5zTQvrgT%2F15hfwiRL4ysNjOklCknyZgdhHHLDmKogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgycS%2B%2BXwVxvWzXwBK0q3AO%2Brwbs5RcSfMtEif0Bi8AfiGnaBs%2Fzn0l1ZFCubMU3XdBqx1ablKik0o915IcXPdFW4o%2Bqk359KDdi5PlJ7hHVuflX9feMD2eogMNMUY0%2F8pAXgIl9ByK6cJZYbQvSjbf8xG%2F4E5K4bjA4ZwY8vT4zmu7s5VKUgJal1gXDUBfl63J9efnbXmoAlwTk85YDQiP5Fz9%2B2I4vSjWuanerOP1HleaKNcmYbLyPKkRKpCTP3E2%2FkqFa6hUVo6BGGMhutOCnhCoPaJwAduVhEE%2BzYAWcd6i6szmp4EiV61J4Ufpfsn2kAX3UtZBQtDYd5Gf5FX8ponbcU5jAzlgDSdWfwf8YVEOr1U3EKRrbJq0K3WDNHp%2BpAsyHsALwnmo7ae0nu%2FB4KHx4yGL%2FbO%2B%2ByhDK9LMHMa1Pzvmne88dJsJNeYwWPjLTOiSkp94vlI8fPwp%2BpIKxmfh%2BpMhSaJUuCTH53lGSKsVH9vIBCo958G0VM%2FCRXny1Bp%2Fm6SdQTpBEHiSImIUa8Eck6N2w3MbQ%2FTm7wNRy1qte9OUd4auRvh3pfnwxuLk%2FJ1cakEwqbYEWXevRlgNNN0IXo1HYvuV0GPVU6Q5JAw4zv5KJVtIwAjtzoXam5kqBMTWiZNGYvqtm8DCI3tnGBjqkAbaZxmUKn1E2sS%2F%2Fa6yxWqqRui2y9Vv4XLy%2BMuDNXb3X%2BvqR02izg5uDbmX1nWvcfPLJ08iYRuIXhKIfhOTPM1qQkj0%2FLtQ810OkMg%2B7qOaERwtvITUGpqEQSHwWLvsYwMvkmYui7xNTWYOYgYKURzB4y979qy8KLhpyDIbGBod9h9YlKwoRw5VkRYFH194bWiIZktWP7Jp8Lc%2B72gk2vcSXNK0H&X-Amz-Signature=6f3b8d911a3a74da584735cb8ea480736799af9f481a239ec895d7cc75d4d519&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

