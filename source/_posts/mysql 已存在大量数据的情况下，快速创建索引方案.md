---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UJBX6VS%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCis3Brk4IshlAQqwndtIvjKBWpWeHOnvIWehiqCyMJNgIhAKcTkZQnajxACRfkjx00ryi1%2F29VJiKDNKYnHv9gxancKv8DCB8QABoMNjM3NDIzMTgzODA1IgyTeGSy7f6mgnZbIMAq3AOhIYpgkHR78%2BaEBBGi%2FD2jjP2GBGIGLq3ssGUwhV6WvlA6n%2B6wFSreilwg6n9TGa6vTKK5Q05p17dIhzmBsXgy%2BlUy5ekyL7VO00dDsptfEMXjiQUsPdpiN4G5zDS6TvHOZ6Lu1i5od68P8VGpf9NJ4tkRVkr8LHtsTxRN%2F3%2BD1NxADY5N8AeWGZhw9PRDhts9%2B4TC8Pe4h6uDclo%2FTn%2BbcZ5weyQrw%2B9dKPTgGPSyRI0Fkp9N%2FGekB0atpdrOVNJoOvmRvejo5S03ePjRMh5YUzh%2B%2FE%2Bh9vbaQlL1WlMmWQMhm%2F3BKycyRw0miOHvgFxts9U8jU1j5wxqVocqdlKu%2FUAzopVTnG4aDiiS3Bjx%2Bo9wdcQsWsmQ1eRN4zIBAHIYsCoAMWZZd%2FKZ4PDK4G7kU3Cevzyi9jd4R0m67x91gDya%2FmsgjbcuvwDNJGz85zpKnQ21cy3PpiOe7QASz3bDXgt1eTLd1%2BB1hbglS8Le%2BjZWU3UiwqxxWmr2X3yZpWJcKCQjpjHdJsf%2FWcZdXx%2FPPoLnDLylnI7wWIXAy20NZ%2BV91h2jQgkOb3jnJNZNhoM8ds3bvpP9z4yqkagblNOSKmZxXPvEgcZgrKqEGmEqvR8Ci35bcGaF7R4uUTDK45TIBjqkAWI%2FfEv92w210RgkThP47KFrKxHfxXzQD0uAhGswPnHjnqoKwY1t2wWuaRcJlwucoZSSR%2FwCBn2w3jAkGr%2F%2F6ZYJqqDwpW0BRx10t%2FNZDKVc2wfJFULxcf8l6MrH3h3Kqu3xCeDXdDKbzZzx%2B5Bwabu0Je1ESrJx%2B5OAMjj7YArdn3W4YPH8XQ%2BRexwae1%2F7ZrgZJmRQGAZ0zPSGMxDMuw6AjY5F&X-Amz-Signature=da2e568fa51f986eab818257be09058dbd2198ccd2ab90e988eae506f459cc2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

