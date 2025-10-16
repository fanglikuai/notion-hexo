---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZT2VFH%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAF0dHXcfCi%2B80EaFvE7jAYnTpOznSd1xgraglD5D%2FdNAiEAgETjmqAn6NIWKrrdWYt7v6Wj3kaaSqpzO0H6iYSTQJAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNpwGojI%2FKmAsBONAircA1KQqxqRHXu%2BSnD4S%2FHThKhxL187JmXIAIKp9vAN4PfvuBB0BrIlmi8Ul%2FGgBvoD5nJTL9c0G3%2F1bCXQOl2R5780sowSYHsYIbQjRY6X6JNl57B8TQHPr2tgMkkbWRMDbTS3CI9F7fJAcR5zsgy1OxNvxRDK6QQKDLFhNvJajSyYlxxgZNwj9bmyIwQV3ILIpR%2FV3YE%2F8nH2N650iFHa3LrexdaYNUbSBHUoDfaQ7cdEeQHP5t5jnO95EcoYeYIyNMIVngrXNk0YmdTBt6XBHUFpCSZOJ8EKP8wrihVlaJCKRJ2tF6kVF2ODGMJQ6Pmsuevb7YZHli%2BtiDCGUTiqFdsEIvmu%2BJqc7i%2BZqNGlbdmvHR6zmFlVA2W73hraUFe%2FO%2BcYj8z4UeztYTL%2BknWxdUx6vNnBYUlcOH1iNcPkeZHI%2FPzYi7iDmUh1zG%2F4S4QbAp0Ty50Bf01VqKBfvh7j1R%2FsXugDzD6qvCQRCHbD%2FFLEHY4%2FPoslAFbFHezHbKjtXHdV7UPOlPMIvAopPt99D80CkjGahxvoQ92KtGZ4nv%2BA6vASlC7h9JR6UIwbYshOvLwnZjG6z8AhmancFhbpfl3KkbTVRM0FD7nIqBdQdAB%2BKSnBf5yId5J%2FLDgoMPWLxMcGOqUBDLa%2FBrydsP0MexdTa0xoBfQUOSoy7rL37Sf6bPIlmE4sGVgN8oFRca%2FJBoMSMyuROpz%2Fv0y2UxYARWq2DNaUcTAWne%2BBJl30vJ6ASKJXU7zGlVU6PyebWUX5YRU4CvBzOKAUqwrD%2BTtS0CGcwNk%2Fwrstm1AGSdyymGmfwwljGJKegUjiRzT4CvigD0PXHJQkbcJg5wEZ3c3KmNSEs4RJE7XvEd20&X-Amz-Signature=e93ebaf0476ce95cae0235780630610c897864d8eac4ff2f2d83e0e88877a03c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

