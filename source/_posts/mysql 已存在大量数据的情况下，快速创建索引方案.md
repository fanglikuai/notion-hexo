---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMNCCAZD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDwlafPsOrx0BcTLmX8J7c394PApPY%2F9rFOVBsNTPii%2FwIgBwJ9iXKP0aNmnTcB9KGSJgxvhWtcbrhBbRAh4MmXurcq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDIKZrXfQfRo6rcK55ircA7qudbeXcGXcKIRKxbj6A77%2BZ9150B9EZWlzTsU3EvE9L5KWcfjWGXAVcMHfgDd%2B8%2FLp6piJ7xHx270K2R3WvJs95SZzTaaao2weFQ%2BPKQ8%2F%2FW1hFjjFom4fwhY90sI12XNDvoSRL1om0eAE2VhnzAMohNGLXNC1b27d%2F9qKj%2BGDssxXhUnggk1Jklp2jsalATeObOrmE42ozPZ%2B3RZijyxQiVGJ%2FFbbyVR7AaLqGW98n6sXuHKIH2KphP3zBiM%2Bvvoa3v1zESJyorDFm67dlL4%2BqoC1j%2BQRREUCwMCM5UVbMDebSV9zadL4U%2Bc5joLx6%2BgS7%2F3qH%2FF9UQs08qcmoy1UDp0zq1%2FEbUDJxRFtTyTW33P0MdLhQjQfSm9Yvsuh6JzNe7%2BDB6S4R56XIgmqvrR%2BtJ5yr%2F%2FleRSjiNBucMhrz4hf%2FGNKdRdipA85zPeYaQm4Nop1p0UvwmTd66OjqsU%2FGimH5%2BCR4TJ6Wdv3XFIEPzZlN1pA2L8QtfUXvDiBylnDcVI8LX7S3UkMZLxrI9kNKWb5HW%2F8mP%2FMqcJGU47qmr08Cg%2FUluTLLeYUXSBU0OncNllCBYz3ueJNBubuE9SaLDG7kFDXNpQ5CE5gVA075jLgS37RLRx0JrJkMNq4rscGOqUBKDNzpOSukaD2hew8VKFyyOgaTGeMX5VE%2FGpNadtmnLMKRuctXouocOKU3k5xXsLA%2Bi5dxcKUudosqpydTw0dc4iE7ivH6wD6TauSRljnHzN9jroz1qy8Hx7R7a1SoVB8cYyYpX4regZ94jyV6rt0NtMyq%2B2NvHbXsUcbW0ulu%2FQd%2FWg9GZnsDF%2FHdukPTmUpbGJ6DdaBn5Jit7EbUlOI8CRojTCb&X-Amz-Signature=e82a210ae116cb447ee8fda6de2965fd245326e90487204425afde15d7f24479&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

