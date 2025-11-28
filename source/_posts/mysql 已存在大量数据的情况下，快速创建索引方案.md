---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZEF32HUH%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T150051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHNloZ6LJT0hQ6r7XJcfEVvMg%2BkABehCTdQRI6ng1QIaAiEAg1QqAwQdcZAe6B3nVkIal0zVy%2FHpxdmFoZcL60jMgr4qiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEvbdxvEYZvdQO04WSrcA%2FPoH0DH8EFdYAvqbiJvMwincBWV87Jrue35tY7cMb5Dr%2FuqIYKdtClOo%2FAEO07LW3UenpaA2jPGi0jImnRlplh0s9MHvc6zjrvU9%2BUMid%2B70EZ8ewSGv6TtduxKtmQTTKcLnTPNM0nhFKHFx%2B6bau%2FQAekPpRkZTYQSa0du%2F1euRzwfjsiOjprOElEDd95VVawL9OXki%2FoH1T21qokAA9UDWyL8QnshzUW5skuM%2FjGG8p2%2Fy41jotYkOOtmNjl4e6NoQrGc6BR0b6ei30qhELeLKQKC7lYj6ucolh8E1dGEsW5uE7yM%2BTHBfhHnh6ddyISDsALFIma%2FSU%2FZJJ3SnCNPpBgpebfI5zMSoxYPlunRKekAAalzR4CdcAU85aiOGxVkYvnp71EsI%2BtX%2B20VpT57kOEKIWM12BpXe9ahKZF1xeuFHyUM%2BEhWCMoAiev1yU0EFShsMiNTTasqFnFXRQPZ45%2FwvJVAtXcyYnuvrzmx8yZJOVQFYWdu5sEdeaUBXzKSeEbDHaPUsaBmJErA9VeiPX%2FtYvBh00mUpTox9W1WVfHQw8UmquJd%2BI0QVY%2BLv2rc3boCdVUXgsINkV2OOzK%2BCF600XSgcKvrZsbMGxTAVDMw3ZrYIFc2he%2BbMOO%2BpskGOqUB18SPAs%2BNab%2B8c2Fla4XkuJldz6dViLBG2QpH0Kwy1S749nJ2VuM8sZ6pQEEDye%2Fwpf40%2FyM7ARtKqGS3xtz6R%2BmSygoZsrLAx%2F9e7QKU9IDkhGjj96WMzE5AlS8M78I8rjzDmHWampbQQU%2FjhcBTcwjJxQKYCvKtdxfaLMK7Nz1OWHW7YHuLmax5fEhBWbbiDZDvdSqCfKI7teN%2FaeZS4OgP%2Bl%2Bu&X-Amz-Signature=b8bb10f1876a8f3d0dcd49e413e60296bffa9898bb19f6605413144064c150f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

