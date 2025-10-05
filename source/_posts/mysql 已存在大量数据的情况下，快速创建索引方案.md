---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YE4SVVNI%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAjhyuTWcfUQ2Zazo9tn2AUS4iGNpW3Zl%2FFQ0gmqErgzAiEApgdEK1ATfHtARBieHZXu9GzKNUslyBXRuqNbeTWN4Icq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDJOFqEgxzBPeizYP7CrcA6m3wShbC%2Fux6oy7jhIBUO9iANEblQLvH4G87wnRnGGUvmKglKAPjY9O7vwRoRVdhrx7pPAPkucFDMYdMVFyciquDg9pvPzntFbM2pgLTCcto6PALQDfVwCtHJfufG2mxUc7XVu2wcxWgfLK0F47Xy8FFDWlEls6JW%2FEV0zscKNQOG9lunR%2B21fJqqXM7nH9CK47yNVYUarovI1FxrTD8EUKnGC3xJohsW1BP1exhTnEn18hBev4SsTcrww9ow6xbpM34k0QEEsYk0SUexFHEV0h4o35lqweIq%2BA7Z9oWDEzqtcN09Ez5Gn4KSpI6jW5giL6BTiWgLlliMWKYa1wJ3vb0h9Md6TKnSf4cGuSVedFVhJOQfxru3h12wNp0RDGIo9EciF7FTRvuqFdV6Y5%2Fd8yO7F%2FwjTAVbfLymMmB78WrLTZ4LEvXR2h5mzap%2FP0zUrTTqzLqnJyKzFf0WNaBD45WkRJIVpH1BZiLEQuVtTj1%2BfOLXkzBbv6qc%2BuFNBxNeCVpXyz8TdZywyUHQlNU6qx%2F8S%2FLhiFhRSyRtRELPgprj50imwzo8OY0y6hdoWRWZOn2bz3CoqrLjTaHQ0qoK89rxQiDo1Oi56T1FG%2Baa23Sdwg5W%2FX1gYxb4GrMNfhhscGOqUB3Gn4xG20bLbNHlqcrTxmdz5J2oWDPvIaFI0gvPZNORuhsf3%2FKKLSMXRNraTfL8l%2F5IEYHvCaM5TNB5UVkbElJ0Xx%2BzxWyXdKCne%2B8NuewvzPapRC20K7ruSkx%2F%2BiUETTqZEKh41wZBxpsItryxHetiO%2F7%2Bdjn7lW3MSxdDVWmyAOE%2BjSpla4S%2FsaC%2FDOj7Dzm6tiAHcCxxoQCWNmL0wOUUqtLcgc&X-Amz-Signature=a299a09a11564eb1b255e2c004abe29d1c03e9323306a3327461f861d4889f22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

