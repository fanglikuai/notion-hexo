---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FCQKAN%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCICP7RqtV7qj957v0qZ0GxreuEP06NLJoKHjFbh3q39NKAiEAlRnTJoVUM%2BNUzZ5XdEfQ3ozLeZYceoLBAVBPtOgJCDsqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF9bPm63QkTpKoQWJSrcA1n3UoyC2UMGdK6ABFqieVolAWHNDWsqDMEGns8%2FKdzmms1b9JkjAWbP2G1emPg8MoSUuV7K1e0qYTTObvoB8YOb3WvQ5W89FjUZJU%2FD3hJ0meondfOiQT4wuIKohDZsU8JO2BRaiK7HK48pG8%2FSSCF%2F7aHaWVWJS07YPfiRdru4Q1jUakMXv6bcg9W6KO7Bpc953V8EikBvcdDh4mJ%2FO8PVYKO1p%2FnJhIvh7r8LWsDMlSlCT4XyEg9DJeD6C5OncoMkXe8SVj97ZTRQjc5VEar4E1A1VTncjRCK6m12VF0YlmuQUiDRc0YOoqp2IrzAijGJdVchic4ppCQcHIXpXS%2FaxxxC7YF2cuJFinkekUyaNQbVXRw2J%2Fs4sAV9P5kuhHykLxeY%2F9GsxXPB3Cdoc%2BszK4uQr8UKjItZ2%2FtgaYD1%2B0O8A%2FeCKovL2lUCkZLI40wcGeSe%2Bgv84pgh%2F8MlXYF9%2BGeE3sMUDb81Usuc3Q3AGkeBOoyC2TCD0VHqA7QAUPVbEYZ8CdqpCqjfhL0%2BkawFB%2BIKyWmJ0j5xat41hUbaw4cY94VaAFdTFd9LopQLRFKw%2BCMOwZ%2FRx3hlyh9PiH28JyXlYB6POrc3WJ%2BstFH%2BpcE6c5ivGZVPBCmmMPmYpsYGOqUBliJ7CMAwCSLaKilqp248mWIfPAXBj9pISkYlphCrWuoUJKsEg01WM9ef9QPDuOWt5Mmaz63WLAVBJyupcK8CiW97Zs9lvJ4wI1O2BF%2BxfELlaYPoC6wGGSBzbQSVGmuG6aWPR2yTP2cq2P0SD%2FawiAewyCdmnXPpIEbXBLL7cnOsqczcONrQCXwLG2XGsFnnd7oAI06tL5yXZxbDLffmIDoY5%2FdT&X-Amz-Signature=355c47eabf1b214fac88ab1b4113df988d4013b8882f7a3ce3d71989c7933ab6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

