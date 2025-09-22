---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSQVCEJS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG0V%2FGsqPD8MpZEd6lV7LLxsg5eUQyFalQb7Fx6ooMbjAiEAi24X9RjwZDA%2F98P0zdL1sH5ySdHoSsZVzwKLOl%2FQhvkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEnubqTZ2K7ywRIXBircA0atExx2rVlfna8QmzfJyREv7V%2BV7J0FzTK%2FyQOuLGuPFfA5eXyrlS5XTaYKo%2FzURfnZY86AIOKzPZUwVh8ExhtwRSx1YyYxNU4dndzQOy1FfU%2FrmL%2BRK43VZoxtFIb0uCc67bTp0v4%2BitLRfKj3mUCSdMX0NUlqroRwxTWP3VqMx%2FsbIaPbnn9Qcvr3djyXkXH3WOeNwHk74HCMuXB6MF1MpFQGK9O9c3blX9v8FFTEZVSGGRgLSwIPGFxGGJQ9hFNjGcwK01KaZY4wVsqyePvDBUVy%2B52D18%2FSzBsljPk57CZij3z4nkG84viS9QvmUIw9p4bGDRhJCmjYjWyMlBF5QUIYkHFQk6GCVvD%2B2sSV%2Fcdfx04s%2B8HdRC9iR0Ya%2FcoZQo2LLcNWKldrBgvsY8PZuZNCcJap8olsz6yPVIAVlwRgfj14bMsn9Bd9VMPBzznhSF96vSyqY04%2BVyy37bsPlwSNVnqia2315fi%2Bak849o66bLY4oYqekePJ8C9J7N6kEVQOeBVlPIr6y5PBtenURM4bNmwEy%2FRTLDWga%2BhJuvRKFf5RoeBRCxa5DtflE3aBCV0VjA0MND%2F7CRKepX3SxCImG%2FKje0DU8C9gdthJvC9tD9eW0Kqq3qlQMIbfxsYGOqUBbBI4J6RWcsYj%2F18TKKQ3MktO78xohInvDCMXjTsSwNeYMnwEJaPCF5oOnvOQy8pHD4kFfUyDJC%2BuhqIyWmeU38BKxOVmdhR8CIWIcEf%2FACAG5nku3rayT4gWqeHmSd11BF%2Fll4KlElk6o6TDhefHmIWAmoacOF%2FCEaYADGt4LgiHu0%2B%2FgfzwDJclK277X6CYpGl4QnVcfpmtnMFy4l5d%2B2u%2Bm7D%2B&X-Amz-Signature=5741d31d520323a74ac3a4226c8f9c0d02d5431c4ccb4b0231c7cc6cacc048cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

