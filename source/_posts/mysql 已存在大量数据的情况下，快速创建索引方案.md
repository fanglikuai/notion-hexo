---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WH2QITQI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7H%2BIwT5FGKC9HNwtzin6S1t%2BZ%2B4mPI7orrckE5g6G%2FwIgVduZoWjNk%2FDop%2Blyg1Bj382Ml6PbsUNmXebEtC7fXxIqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFIYPEIZAXhjeFl9wyrcA4gI2hs0kLDr8FR7dJcGOphASR36eN0%2FQtowK7%2B4e7bJgQu%2FB1UJV8aMTuPEUcFk0Xh8U2hdOBQDcerrnQY49pLRON0RARBbXGIfrAQg6tzF3B6cnXguh%2FfVSG%2BXnnw%2FoE%2F0DvQlTjPulEr5aN7z2BrJyvg3sCteul9Oo30YLorVq3npyid%2B3y3%2BAsiB04I%2BSmp4tevb2GZL0qCjtF8qoG%2BVAvnSM6kELu6yMOxHqKi5EnTkKZbg%2F8ABkALiAKIV06M09huNv7%2F39UBriK7L4ErMHoasUsWs1BtpxhPVoFzJvtEvjgIhkFzbJxvotVurD%2Fhj885xir1BdS8bVCJimb6XIZfv8zX4TYuEu4BPYOkxtHgNQWmjm%2FAThfJ2qKrUZZY3AUdZxBffLWR7O9ITt0JvEHIV9JSqUGFCgL3weU6%2FTm5MDJMfqj3KJVbhRKjDsrsvDC1cROzo816YXHEpyhw6e83E9r1xCP4rA8%2Ba4%2BLzxSqLeA3O%2BUTUFfyRRclmadPNbfqKHiAyZRQrPjyIVmr7q%2FFIwZoV840BUbIVWFF0dsswh4zUytR5UF8RtMBAlBPfa6566uEJjv%2BIWaauAbD%2Fe%2BiRfnfaBxlFbbpaiT9%2Bmwx3Nim9PKj7kHT0MJmx%2FMcGOqUBkIutZk2AU%2BYfF97Z7GUcXtaVRWqrxEGMuRaJM5v8kS6G5WcvkGikay9GUrBPTOxqBunZnu%2FicnMjXgqJjShqpfxwfJyljEKCZIdNJLUqPQzQ4V%2BC2bVgS7hh6VtZjkSkIt4x905UKt3pcJWOyqUx%2FkHdxDbx9szh4%2BmL7MhyyfwRsFM0hucO9SBnJ8IQ5cDsMAOrnxWGHt7URNp2MVJTWVZ%2BDKDO&X-Amz-Signature=caa36c733dd9e6a8d5ae82bd19b8fbb541027ec9c918e8b670738a53aa73bc8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

