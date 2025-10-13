---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637UPZEIP%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWLP6O3KOc%2FNlN0zLtZsaOUEODTyN6lnmU8m43n%2BWRfwIhAJ2kSq%2B1r87FD5bW6cGPik7r719iiN2KHhuaJkbtUhbWKv8DCEEQABoMNjM3NDIzMTgzODA1IgypUP9f6tnTBnvuFHQq3ANFMtck5jrTqnqHeprDwDUeTMReJJv2PQFxSdgKLHuZ7i5I8fwzBLzBp3viIpcOxCQ6GzY3KJ%2FyLAbjqmkHeAVhcQtor0Thk60shYvV7Q7OJkgF%2Bo465kz5IALzvqFwXvm8nKU9kBwCW%2FWiMbBUPepDq80gHx71nLasmHIq3tRG%2BbI0TfeuyCOb1GjM%2BeTH0NwuFpb%2F7t0zp61RjHrR9R%2F3BPV%2B2YPxfX6S9%2BXSokXinpJDe0nlRHtiBU4L9UFqAiT%2B7BlQbjGlmTke9wQvxDW5TZw1qnSzT9D158BSGOYTrUxmGrwpV99FVhOVp%2B%2F%2BvkTQSNnR4%2FBA2bZhYxzqTUfbuWTMxbR98s48PT6nLLPw8EHfu9DzjUTSuWqLz1htWZUaalRQredArsoWLtVlE8DQJ3%2F28hu3ajWDc4Qyg%2FBzvdhKcz6EhOICcO3bDN5RyOP3UjBuOsNLiqMF%2F6zyfMq5nOCbUqryz5AgRwUHnW0K1PMWqmuhbIgdZ25zLaSMnhzcue8d7N7KmB6XPSrLNXLGV%2Bu7Pgn4EG6x0RtmdBgd6m%2FwJNu6weN2mooCZB%2FowWOiNVIePSVEOy%2BQm5TB64jq2UvcTWwLwSZz8DsGCjiV88wq99VTPsegby2sTTCT4bLHBjqkAdW52geXZmLwwKX27L3mQlT8r%2B4SFmNzgTQzHcNFBbZF%2BECzTiBlwoSh0Kty%2Bny0AcyzeaKeRDMIzHLcMsndgk0tW1UiNp7jBfiQaspVQsWHvFqE6D5zuiYU6n3QnRAL3f0ix25IZmKO7vITLDxPQpaPc%2FignOlCEPM3IocyoKjIY6CK2go3ZdGnpko98RQAYPeDoYLOk43d%2BAATa80AzI0aIfQd&X-Amz-Signature=1adbb25c4e0ec4b477163475442cfe8af2a923bf24195aeaac129381885de0d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

