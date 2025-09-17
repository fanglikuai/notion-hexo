---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTEZU7V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIEFuJoTX%2FypmiXP1pwaqZTQ1HyYa40jHuz84GN4nnsY9AiEAqn87kikh6Mfm%2B4c7lxHQHe9UN2%2BDZ%2BP6t3tho7NwoDUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaI%2BvJY%2FJxQA2fcZSrcA9iguulaXp6eoAr2MLAkGT7ykCveosTUpmd8qsUGVOXoNX9BdGM%2FfJ%2BN5yF%2BYLFNx1gBFTbuHMkmnTUUXCuHh%2Fe5LJGCVUe86GXXSa0YrVhQQkkerWXjmZ4pTPD0XlAIcoTKjqM2YXB2Z4NXfGX7UTMBjEurF%2FRr3oMjrHxSMxYOZLWqyHmy2m73T8n7U%2Fs%2BerAO6oT4cs5j0oXIt5Se%2B41StLOQ4wqdj5EsZDrohGL4hsohVXu%2FiaOD0brL2%2FcLNpz4ksrOB5YLwCS5nJCJ3A4uxg9w5jBR6zHFLiEtuOOK4yZOF8vwyYfikD4JSdJAu5ne8L%2FdIt20KYPMkChTV6V3jirntADCTz5bauupxy9X5h9hzQulNADK7KFKjP3LX4xsUy8OJgP8dgw%2FMTI1%2FhHxQw%2BgqKOwciQOghbA%2FKyE8Z7S%2BmeBFGnZf8g69%2F5SzWVR%2Fug577pUPUxkwvAI%2Fd7LUgSVgu9LEKE2JLJqrqygt%2Bx1GLFmVwHIYuoeGeT4b0bVNh%2BXJzfu0FflRuhQYrXrY%2B6shCk8aDAAKbjJp4FLGy1rpJkFAXitVeRtqWsqXM2LADa1rq3VLm%2F4kyMn%2FIn4X5lOSmhy2Qsh1HfNDSrn2tFDui0RYQFCEtBhMPrUq8YGOqUBwDCA3q4yhj3CxIkXm%2BbgwROpBCliiKOMRKzB8bnCwJpVGuAMCXndn%2Fq3DyktA91zY2HBCXw5SlDcKGb88CXaKF1CpSCjKG9ATj9dR1CzQ%2F2chAmDtBUfAZtceHcFE%2BEMPGMX0VB0SWxNOfhgLXgGg8yB67B1PpQcSncnnlwznLPeO6TgqcmX3%2Bk42pYgesQW%2FgqVFiB14LqU0HubpyJXLo4UYNPP&X-Amz-Signature=96db441ba022843509ad84b9b5464a8defac1fd524913150b6d6d2b0a31896bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

