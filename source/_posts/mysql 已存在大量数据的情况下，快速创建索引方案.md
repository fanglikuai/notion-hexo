---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDD2LZBO%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCL2NEao7jLZigIq78ypr%2BHdMC3nwcsVASunjoEflaqQQIhAN1pRAd8jlCdm%2BVN2%2FpqLyAjSrA25gsR%2FeHHcP4p9hhsKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9wnnteIA4bh6S3bcq3ANwnYA7dDjX2UKNTte2s0Q6l%2FLPgym4az9i2AdRipnNWs5M9607TW5cQq1YuWA%2BJ6ZxifM%2F47MfIT0F4yMLcM2WbEYbhUaSGtxsJXBU3GK35bPhRFDuJwVY8iy5466tHxOUjkFbYOX%2BQwp5bFvEJyguxYTMqsXtGds8O1AJMOhzVIAPSi52cngt6oNnJGrF8FG2o5%2BjqI47GNvAK0hQG4MdHZ3Mp2XWDoHDvmYTz0huY4JnEsR%2BesrrEFYuBxBvLg%2BV4qtMK2VzrncP8NFU8HdMvuafP47kxzh%2Fi2L9tPIjTaH%2B7ohbs54VOJ14HRdmePbu%2B6enIp%2F2UjPTc1ZnJuk7NydOM6VZbZlg29WqGFPpL%2FMVNNEDXYmzvohmZrp7GxK0PB66oUCIzeaCfM8Y5WS766qAy7%2Fm8aUqBygjV2F29s2Ey7M9GXVDXpiiPSg1IJU5Iffw5bboVDqLIGIy%2FSiLOlUS%2FFq3we2Hx783SUY1QYhxlVyd4T1Whuq3qJIKV01gj8%2F7OIbqgHuxppLq8kKrehXPPM1OKYGu4bP2iGd1nB%2Bwp2ehhjbnXTatzTKT3e8TwvLe%2Bc4of4nEJTSHkUprOH7LMX0uZl7gv6hMvB25Xegz9HyAmhV4ny%2Bj9TDb46vIBjqkAe5UyzLwSMB15tmg1Uodb27Q52ueNshpTgUTTqZiLAG0iufCSFURwzdJEkKRArg6F3P0FfcU%2FGmsSFC6O9YOsrSFqnl0a6C5MwFT5eXL8uFYiyPrlUE6bZdaDCfbSLXXaL1RpRuP65w6t9Xq%2FAh0GVNVrFpuDhxVmiODqssUXbf%2BmPuv12zLW3IvnNxinedmZ%2FFhFRmtmKCOItwvaPBvf%2BVqBTRt&X-Amz-Signature=d46bd148b2e8026655087df8ea42d83a97785a3e879436dc597eabd9e5e0ece0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

