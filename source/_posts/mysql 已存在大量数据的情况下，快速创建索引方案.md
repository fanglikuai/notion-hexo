---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Z6OJJD5%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBcbEbUvw6pGyxiEDLL8z7xjGUQ9Ze9Gj6bzzoQCUTjZAiEAo40uoe%2FRsiF98lr%2FymUZw5KsQPSVxYjVtQ5XdYFPFi0q%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDHI2sE3PoBJ%2Bjf229yrcA35A2YTk%2BIWNFtnPuMzZJ2RKlawuCbtWQjojf%2BsE5wNazKUHWaOVfPBIGml67IbsxfYbcmZBjYQU%2F6m1jRy9YKf%2FX2SxWif7CDjangoCKYB5WEdhi7mK%2FBmFSvw8VtOnoP%2F%2Bw6%2Fho3qox8OncUdsoXVHlbN8B7mIPgUQdwwQePKEHYX06eGCEvOD035ItJK1MLG6l2H31hJQHQsWkc0fV7z%2B1mHwOADSono66i8UvAWuso52crDUTPeA1owG9mIGzc3DPOvWsCiOS%2FSN1GEnLzJCAK2IjBK7rIAsEN4K1lA%2FXur0o7OJ8k8RnpJZhsYuxsYIoxO7lFp20atiZxHwEnUzRwQ7TRHojvHO%2FjiE5dmWdZFA04q2PaAHLdHlRlaBYGSIhrZKNaHVq1k06VjqotM11KmVGYZaVHCcK3Khwfz%2BdP0GOwOh9xRrGzU6IE2Qr8P5NDJx4BuJBedPbqkzwxBdReQYlfUmP%2FO170%2FfiHAr4Tv8oBZbTFNEVGq1M%2FARsQl5COBjoEz6OPRZl4nRX4yQXQ64jj1EVmftT8l9iETO%2FrbuY%2FsgRX88TCCDTn%2FoexY%2BfHEtmMakYwxTSw9tdBIGlpKijT63pkgF3Co5na5N5sCqNZqA5%2FeDeRvFMMTNzcYGOqUB7%2FGXl5dx%2B5EayqPSRLCvgopJf3Xe4VtUPzNtkBLAT4nONe4QyyoOgmWfyQ3bDHAFv0NNhJ1yDIeUtpGz9ldq3URkdwbcC7Yvn3XuC4vMKxracLSYz9lMgXc6rjkl1nWdVtww1sbfdLQw48T%2F9nMFsDxhdGaM9ogQFZ7H9deY%2FkSVaQAJBcwQKnkWyJtXjBMjvQtPJsK%2Fm9LJd0rhKHFxrWqG3E8S&X-Amz-Signature=8050355f0cbf694289b92f6c9c3433515f7caeb41ae3c333fbc80f9362283a27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

