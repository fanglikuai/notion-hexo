---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Z44WPFL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T140111Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJGMEQCIA9lXryYO1%2BIJr7HQW5JpwKNwJudN7A5sAc%2B%2Bd04nSWxAiA%2F0qaCINAFN36y7v%2FyK2ma3TFTDFEG8TaXfE5ms9uPfSr%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIM1SIfSNWOHsdt7iCJKtwDhm%2F9D1oIS1M4o1wfq0NEFPDTQ2zchmpTbhdTLYIlUXUmRGGH7lefVDWeNzlkKTR7wwfPTb9XIGZpW9YsZXCHBxNCtu6t01eXG%2B8ErsIweo728pEvWCjnns4QO0Ahcp0TJuZ97Wu7qHhRz5vkKceNZBKfSogZrvn0NvzBdKJ5KXQsPAj%2FzTDapRwhG5ZyLynNOQ9XH9Uu351CFh9k5t%2FD74P9LCFUEwf756bVF0ekuLaRZwrWoQXlHeb7pe29MrX%2FXFmjhS4qx5fRIIZEmtDidAr48c6O0sOEAXqYmiPgNYMrahky89Ntjd9aEiqcR1Ukl5tMI%2Fq6OJrDyfQ95wAWucx0eUwUX07TaCT5dU27XmKGGA%2BM1qB3Dg6A3izhDOpADSeM7f9KmDx8wuUZggp3pFNUzb%2FFlU0rum4pV6LwSiBY7NvE7vqU8PFzWM6R6UuV2iFtAG7X%2FnL7SHBIDomcSAtgF%2BZDndU%2BZ5hIET7uo7TsAS1qIvfSVTfLf6e9m0%2BmvkDgK41bdsiJ2UFRQVvj%2F2cmM%2BHoSwnxAMdFipWqmyIeS%2FZGcKew%2BZqzjZ%2Fi2Bd825WA6KkTOnPEW3JJ3b7Qib3IgfZhEW3HCOzrWs9tFezzlQ6P64iwJWwBKJsw1O6cyAY6pgERvxuzapFn49cafC%2F3ZpXHGMLt0Ew6ehURfqNXUuw16u43EjEZuQltpxcBx2T25jj4nSSFoLxJ0REQv25cz6%2Bl0YRcoHd2xAizDsAI3esqz4IMVuaEGcmaEoas2NXeLGMsZ5RgbHEMqOr1fW2O%2F%2BNOJKimD5XCMrs4X0xYgXcpRpKRQlprC%2FhbLk7AnZUn%2FJEWg5jCMRKmTTzuRpJWYjQa2fUTWnsV&X-Amz-Signature=54b93acb25e1400795a0a2c0b878716c4794ec93026651d166e0ec0c7939939f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

