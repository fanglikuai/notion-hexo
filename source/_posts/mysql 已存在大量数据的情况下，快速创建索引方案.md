---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LDOW2R4%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOFc95UxvBPloGCIqNeevGSGdbM7EShG%2F8r2j7mcbvPAIgFKrk67QqG%2Fn1DnHeLpe8lydrCZtxiSv6W8MsTxsmXvgqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDt6mMbmIvqE5QcEcCrcA0DcPDGfsl%2FO09D2XEk3dwtr6Wog4xpeFYgHtPzcrBexlaLE1ZeE%2BD8AuV%2FsA6ZfNDESnoBuRMeQNj9NI7dP8sasGBq%2FAcZ5iZZx3dxo8rRwzwJwSti99C3Va%2F3A7SD9B1k%2B73U6ZIF02vEvveq6768oGx6qZ7JUPioT%2F5QaBtw0N1YK5REtqJ0IHZ63CQ7iDlTIL2mhOTMkpp2PjhdwGEycF3XfOv1Qp%2BPtHsRPqD6Lzf%2B0hkg8NtpA0YuMKeeUQbtOZbMb6J2uyclAIo%2Fdl1kwrBIfY6e5KT01CjhaP4JfLRgFo6WrvP8%2BoQoUt1aMjXw5qe5fNEJkvdhjK4O0TNpc5RbENqRN7bvmrMuuTRrQUeIPkG48yiasyFVsX%2FVmcpEvjtcJaupEoycnkiYshkjqTEGvs6G%2BMFZMn3dxsVJ35YGS5gqu49e8YYwD4A6klwYeQ2jrG%2FThVZtr1Hm7sn5IByznVnNEKM8bbH27fMDfWEaxO8jgydrVgxoyfewcg8IWYU81xoMMSpVmMxsO6sxNSncJtqSuqGuLlHWoM1IS5nKPh2JY6u3PCf4gU6fltiSvOfYWjs29%2FD8SzJO8go4VWVXTgoEWWQNiUwQ942nuBKLpw4E1jgd845a0MPzCxscGOqUBUYoaXJGTr1fmNJaF8%2BPBqnVfYrN1EawFu71s3vT0YIKfBUg9%2BUykGN6MukezO0ApqzDqgN4LC5rf9BbuK7SgQ56e30y%2Ff366SOMRDXUlaIGy3OMKgABRPAY7BVIP%2BMYrVTBsHRobchSzCKqlpsLVQX9Slzk3NkOM0ekDbew9XZTevJMAnXeVnly5c3SsHeFVhFkSPHVNNdzpI0J6jEaL3n8YcmDg&X-Amz-Signature=58ec23656e8ac4ef5976a4bee12990b24ea74c9f014f8fe01c856b7e1b834902&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

