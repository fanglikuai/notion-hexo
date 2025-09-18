---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HKUNUME%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCbu3VtJJM6nqTK8xUSnz0N8LMAFYY3oXOHn2BNElIg%2FwIgNbiFuQ1z4%2FGMeSeA9WLGEASMA8hCMXPTSoWl%2F%2FXijQ0qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLVXt9uSM5Uon2ZlrCrcAyp16EMvOc9D6zQhIM%2Fot4YTDk2MLxhGqUqb5BXnpxno3xAp5RbUaLMN4MyQDC9DxNKyFu4MjGAks0BWwa68uC%2B2IrLGVSHcEKkq9lbL%2FnKoXCKHTdEco99k1NgKE2hXGuTdRfmgC1OUowX1Irrym6%2BLi4nkFu%2FQSPLtcu4vpeM17%2BGEDGOPg%2FYoGff4v2oaCpsABEecoxr%2B%2FIqy91%2FfKT0YSMZG7De6USg%2B3dkiJePfVf9XRSVtdIWls0ueLxkGU3Ud%2FS7rKbS7Fiv6YrfYmLskAhM8PpkcKUE5kQiMXvBwjavli8YQefz1Zg2JwF1Olx2LLlkXSaEcCicEdqHzm8x42AG98OuwYi3xLp9d%2Fh01y4kg6dnO4jK0aSOzE5yn4ANMF8aF5XjlighaEYn3B4lYG%2B%2FhK0dGj8CFi%2B1jdT6evvHcKReBX9M8EUBR5xYTb9nUzwBnT0%2B9JS8u1d6SaIjncQesLEPFbdOnklvKVifHRc5ra7YOOSTl6qXQ3rWrCdsU2hGBje4CceAFESU0RYcTxnhiVP3%2F8SOVUAtb%2F3RL7HOVR3jUOvndgvaw2scj4ucJwGEOtUMFcUdCQMr%2BhuhplGqEFbyXZBVaQZXXre5GZ6%2Bo4q7l08RFNi%2FUMLe2rsYGOqUBTfAMQV1mtCYB6bG%2F5x3HBdcr2d5i2E1jxT30eIwg%2FHsimMUf7%2F9hUhGDemuOnQgEgPHmD3oWj5L%2Fzqkd8KkdqAz1KHMd55AXXY3nWddeJb9iSp7WqNyOIOdWQoOt4ccvm2n4tFBDesTBkHsjaxCzoEQb%2BrFxb0loNSxcNR4nq%2BvF%2FuTQAWMnPJsEz%2BpNTLXLvFBuA8pRbKL3EAdl0f4qGDtwAvD9&X-Amz-Signature=5a70c1b04261eaf4d6db0e673210a09250d5c7c8887000fffb71d88b4b2c3aab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

