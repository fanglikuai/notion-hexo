---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673722DDZ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCm0hmBLsu7lqmJW8gSeskxi40GCZ5vznh6suIE%2Fcy%2BMAIgVBfV4UgCIgKGSE%2FEUhrNGFoFV11aSg4fL84PzOnkFtIqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOjNEFRswgCkrbbDDSrcA27b0goi20pBwCcJSRMMkxZ2PA7NHv34lGPuruXLVNbNdWQ%2BPZ%2FC26kNVG%2FITUebFR9Q52hZE1Ovm2F7txwTIeLaK4F8CxGVRWCXFX6JPXMyd3fXF57EoxsIsnzZA7JEgfKhQYglvTeiLNhHE5Rt64vNOUlmFXyHU%2ByxxOw1YTrWfgkmSSXtMYrQi9jUL8oJ1o6W8B3Mc2dE%2FmRkPORivuOdXHAO3xdiJCcfK1%2FAbJGqPkSFtDxE%2F6Pt8jkD6ExBk8ziePWftx6wS0xgQig6i1X33jwgDFhfbJIRWnii1LzFOTLWOuhdF7dkkMW%2FiWwiTfrHApUlQvt6MeO2cXKgOLOe4O%2FLSNk8T4kvvW%2FupJvkDVCZo7vIsNfFWiCVZodOZLRgJjXU3JE9Gq9tYcdv4RUzWleSzUJXncy%2FqjEiOXVR%2F0zEplB2QjjB2WsS8svsr6nOULS3WUUVJeQZ%2BFpIj8XPBXqOCixodfJUHAvwJX6z5HLwiHaRZqBz3GVwFV9L3OyDkDaL7AZnQ8pDHPCPWqpSNZ35PuTBz6611ombBv8XO1tQDMPA6fCd0St2p6qzcdCwU8C8qtW4ggsbzjBYFLuFNfm3mjdkRA1Gtw3xH4H1MzENPzpHv%2BzCcwpeMMy8oscGOqUB94%2FQzNSwRsno9abHODfgB0HbPfVKaawC3wKPkqz8H6EK3TmZk3hU8fMsvwLI70FjqTt%2BDYVbCLuPL25aMVmjsSkWos4hwVuhtBYhpj7RkZ3MtuhAgEob2PsnrXz4OVjry2KyBqaCoEGOmogVWpI%2FfMXEA%2BjK3mS4jUp70ECX4%2FLQx2WusfOBbtIEXBIWvLUbGK%2FU8ZHoX9Ey4JoS9pKNvUnmY2Q1&X-Amz-Signature=ad2a51148c525e97ce134eb9b16c02b93f8565122cf0d8d4236f6166739d928e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

