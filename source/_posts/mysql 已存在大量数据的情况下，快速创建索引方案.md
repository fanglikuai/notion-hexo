---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652KTZECL%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDoyzbYPDW8MtIHoOb%2FYWp84rJ5INwEULFaMuxu6wy0wQIgMn81sSH88qAcuZcFIpL4xP9kZB08z07eODycC6AfGNIqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQM0sN%2BmlD6lf2NHircA6enOwQ2DxhHmdCCo0gTIVRfNHK1OeOxNscqW8vdpGx9x1BRtHjNINBR6hqI8pj4b6effG%2BTMZK0KIaVtzgG0Xy71Qc%2F0AShQ9iWTr2gpS6EmhsG6ZiYFfMX3mbN41ML7hjieLZR8WwzC%2FC3WpKFeJ4xICe%2FDpdBUSnt498eCJsO4Z1SNODzwIel78l2t4XzlvUAQMkXGeS8BJwjNBGil45c%2B7ZShy9BTXMkzlHVvovgNDeHHzTMKiSDqkxgYpOailDOH1LQ%2FMs79lJAWTRVLtzeSm%2F%2FhWIuhVj2CX6rQys5tcwVJS0i7sWAJXgIJXeoF%2Bq%2FRfeQVSbqJiAUDilrT8ovdtH%2F2sHY5KltSHi5XmNqK7ILAGNau5Psdf5S6NGxh38d2wSDIyKm4BlajqBpBTJa5Xh2HKqN5tL7ONe689j0Ao%2BYaBRR6OnWGMshw4bmGoCRpodSncKJGgUpqDTNe%2Bx%2FbtqzLH0jf3hAdl83HK2pVqTPjun%2BoxxK1unDcNhuOv8jHgjJp9COw2CMkZzFrhM2av4uhoRA%2B5mSMgNdJklxvKzO2tkIQp9tHqL2nuzPFcUzbz5sLgtnXXVzhsrBdFhLwkq7oXFWH%2BtoDqFydRjC%2FYtdxyDvv87QnRp3MJez3MYGOqUBGKDcehQYHTSqUzgp%2BCErlhkfpmi2K901Sl85oPQolQuJCN14xZ5CK5BZklTUmrEbV1pqS%2FXNvUta%2FetjMcAPGu%2BoKetPUB6A0qkoYUutg7AbAezxW9nZMvprJ%2FvWycV%2BZHGwckcc8d%2BX3m5JYxh3gpsdA54taScQIysUrt%2FIopTr%2FTGlzuWwqqzOntrkEBgK8GmNMeU7yFMBkMpcn%2BkTjiNPHlrK&X-Amz-Signature=9d10bd73cbd19f9b82969cd5dd81d7fefb8e4909304c80cbc8d6061dc3c2fb89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

