---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3AXHV6B%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIQC6%2BmatTIsI9dGHgpDinQyHL5RZOGRXWTlXQkWV0%2FiYagIgVWhb4PvPmHbKjtvi4u1G5c6QhKjC%2BiZCiAO1OCTvNLsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE%2FtMSvgBlFT9m4nISrcAzWK9H7LL6dejWLXed1qyecp7RTtS1dzYq4KxixTsd%2Fso39WglSP5Pd7nHiyO1eKt4qIjW%2F3rev%2BbvS5BMo0%2FgQN7kRtHUe9hahXTxlAWWuF%2BcVgl5CNnqFMZ%2FxituBs2ZCKLHtkLPzR1O1lA0ibT8riEyMEQ%2FAs5YjuZxS2YNN533WebcvRkqR3jTzrttz3Kt%2F9QTAikgH74ie2GvnqYyLW%2B4hDC7VBh5d9rDnj1aCFMvht9rfTCYeDj3Wkj6NOawG%2FxeLgT0rO2drh%2BXgLli90jkR8r%2BUL6e%2BQV%2FVHJrtz%2FWrF5X68YjodQB1HoEx%2FHWUxBVu0yllfSxQoT9CnWU9TgugYCfCtQPwjC6lX6lk04%2FmZXyVPK4qQP9cSEPotvn8HTzjhBtH13A%2B5AydDtHkcwyDEI1k1AbEGmsvwnpZQAReRVcXV1xLsStsFH4JITIrPFQh7dV%2FboppJVI2vQynYK3wbLXfQxZ1DqRgfphRWV0%2Byldb5aOSCgOz%2FKTCBmPbVWaBqn3yL3SndVeCPjhLYdYSygKUS7gKaIxR%2FZoE3FjJvx0sdOtZ9a5ESYxpuLQp4WNTH8ZstRC%2Fn7GktNzT4Un%2BkS7GPeIvMNsJMmMgpWe%2B37kS2%2F462dWn8MJq99MYGOqUBkw6dGgZxWsBqCNj2ECBjL%2B45WrvWSV5alZZ2lcppUaVXN5Stp18V4OLhpwmRHDsyOACC8hcCJyNGg0N0P9tAy8zHoVPCaoyC9PIP4CamF4pO%2FLCQD419i1ZZXqZNZkBFs%2BXI27oXgvb4aY%2BYPWSI0qGU9lFkrU1on%2BnCI14jwkIifKpXaiksL7O1AlwU%2FHWeY33Ij1%2FpW7mfFKKRw%2FIMjsLad8hz&X-Amz-Signature=c5ae2abdd34b4a542c786e18f3f170021485dd79eff974ea56ab579f08995f08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

