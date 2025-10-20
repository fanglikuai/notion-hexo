---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665KMBBJFT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCICBEsbWb2TJskPtLl7h7I4VksPF1YGUtO2rcXDWueVbTAiAb0EpofZUxzcHwaxYtkGWbxxUwn9vl2rtddmNpU5EabSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMt5OeOmCzyeeXa1RGKtwD0pYbqOuUFApFKBds7cpt%2FbjerDLs67d3jU1SKCwaSV%2FGXJ7Bbeb6%2BCG1cLGxUtGAlWGEUb02bNaKncrK0MGM1ErLZWyW3RYdQTuvSjom4782AxCNzplnwyXHbwsVeJt95fh418CRNPSmOav4sIrJ01Lu34VnhOcnPfNCd%2Br8qj4eQcadJV2ok0o5LPM5D2xClP%2BC2Fbd84kBqyRlu7nuOQkFV8maDu0wm6S9qA9oUeBasPuNh8xtms%2B%2FfGkn1dnHL%2FNmSqKrvQX5VpwhvqaIfyDZfc%2BSTCD%2BaOKOnIuf8ICuOXolIRfkz7JWHaEcGawDjsZdkd%2FNzxF9y7DcTgS%2BcXxH79%2FP%2Bt7H2zxFf2A9UBdS%2FnK53BWO2W7V0ICqtZ1J5QC6NuaVWFCGCbNWYUByIXSp%2B7vD57fhju%2BRP8uo1IZXDQN7s%2Bo40Wivu2wX6yssxPmi%2BlCWp4NaOYIjubE6KMP52uugwGOJq9tTf5fUxvJfrSEFT0Zzgwb9H2ZR3EGFQWzCXLMkZfl%2FuZI8J7fqOVNmpnU3rm2hUXb3R%2F%2FwU%2FEiUtnx3XlQ1yOa9snn%2BeBEDHlWJoOhPY3oP3Bu3YhCuwEg%2FR02dnCOaiSfvaAga4BtQrwWVz5sLAwKJf0whsDaxwY6pgHmebuWGr2PiGh6gPpE2iib1IZyz7e1mLfVmCcYgFGL4UXZDnixJo70KivS2NLPk6sTEwakJ6NBE8%2FQP5WxgervXdhNQRrWOPyK%2Ftjr8wrGfS3OP43ixpxD6QXoY%2BbwznXFsqgPwuExtyDVjFJIQykgyBG66TOCtv8cZsdPswlSbM7IhYzDqp%2BFdOfJY4Tg6vHFOly01D4kFfZ9KyuRtk8QeN6GGRKx&X-Amz-Signature=425f870fb60ed7eb2b64246504d66a534652777d02efd6249dee6afdcb2cb361&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

