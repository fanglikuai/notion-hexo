---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWLNDOQW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhr%2FihyR9V%2BAWI%2FrsPBPL1Nro0HRQZKx8dpVQK%2B5AqfAiEAmZGa0ZgQu04AvhQVC7vIN4mVR8Lpyf%2BDPHK25YjQw%2FYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN7lTFOV%2FqsfBPyySSrcA6WTHhEtv2GtBLT74AEJbK%2BopF65TwJac53pn5tP%2F1YV2OCJ0BbEaiq%2BIdfq3SDeugHywZ9HMWshQw8OCo1JNsUFIM1C6o%2BL8RdGwl7wwU8HHUsrCrL7B1VJfbKzH9pPhdtKDR5ED3DJgaSRWKHv5WfOBCRbMnhVdmQd901hWo668yBu3QRE1zJmdiPMt3CcZuxy5deaPRnAGAZzyX9fMWokzz%2B%2FigJkETHJUjmHAfHsX0qsQlgCoMMQU1980yWqv4UKRGJyCeFWHdzzwgocztN6O1fAy91%2BHnK7JL%2BtsCxgUmC2yF7KSdZrssxD%2BJcpc6RbUfjCdF8npebCfGGXmmbypeJ%2FYNbkqbfbMWchEFRSu4yXaZwSQi23NVhmBoh9dFNX8yyV2LtEhHnuyCgSei2oCWL%2F%2BsKtoD0CkgP2iTh5Sn3nlMcsPREU1G9o5jxi4%2FxChv6KnSEF8onOiOYQJrJyNk1gk4tTfuEoGu8268cHrrGmw0zMM7T%2BwtIktQd10sueXkqe3rD7aBSgr6qmYiqUmN9YQgSY4a6FZ505jzm50bBYdw%2FtEk9UPXlq2%2BY7XIrS3Gb1w90s6Sl4GOy7P%2BwxRhcRoVXAHOhE3qrfHZ8IZBLblwiWq4ld6ktoML7HnMkGOqUBNSidkjR3YV8weS3%2BtY4Dyk86lFVtlZZFDdEtXhWlzclOg3IWVWuHhclOdRVjJu214SCkavC9rrw5u1zW9zptyXoJdMmPL3jGRfo2aXvBhvUqFjhnMj7YaPGK7eECSb6JNc2RhjianrZuWGyRFBOtD4%2FGWhqAzwS%2FblM%2FePl5Br7v2jA7nqTwOcIOH4X%2BLRWx4HNnCdPC54mZKgctXM4Ke8ze7qa%2F&X-Amz-Signature=f92971632f33032b1239ac2ee739fae267da5d391372f4ec38280943d41a09bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

