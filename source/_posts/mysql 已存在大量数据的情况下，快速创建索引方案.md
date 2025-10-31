---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LPKP2FP%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQDkyZWzrfseW2zOxlAXs%2BH0%2FGIzmElii%2B01%2F5QsJQmBBQIgbNv4F%2Fc49ZEtTuxTfLZ%2FLFukSlaPUuXHNtRZNA0hM64q%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDEiV46mRnn4gQnXsmSrcAzRJAMEN4cBNYlI8afDqAxTsbzhKLe1D8gc25wNmVLtFn4N%2FZ52ZXTX05n6w12uGakTItl4Bj0SH1tMIBpTaywhrOAcB%2BTay6MgnsckQZ5y0lHi3E0pXkZuc3WVFxSneWW57BdLu6zDf3iCAvTv2mT73XKKEDJiiEzitkfekdWdzX8UjYHtsi7DoBrDoD3eYeHp2NsD7%2BRzL1va0L2DMlIu3XjZB1MAebDgPjrRDcDfFYFVTr8EDUO83RNmm3hVlV%2FDy8E1kIQZX7h1whAgK3MDMdm3jLkG7J1GBGrdxzl%2Bymiyt%2BDPqm6sLSSTwdhecoUsDcB6KPKKsHRrSRmlZeAb5EvFxK3%2BHOaNxj8NRsvPMIDr%2BjTpHFtFdeFsnoTfFLW4W4fI27QezT%2B8Wvle%2BhBsjCqy5LCiv9yCNpfUMWvUZz60Te9SGdvAzo9sIKYgtAehRkiHjD9urrYO4ssXIHzUwVSWVItzYpTD7JtScPALbVXe72X1MLiXjxMltHHcRkDwhktfQyHsdOnjbPmINuAwrxa4NTKH1faa68d28mPbt%2Bdt3QiRQxcdPyCANJGlIRZrZzUK%2FOJoYpyBLlVGf80AzOTkW4S6nf3OXGz21RBQCzFjNAGRCX2lSmC6yMMjqkcgGOqUBRNKMRuPIoRwockExnP4vI0%2FbYA0eimRytj9Ylu9Ae6%2FJ3fqh1rO40xlzebnnqg1KHquthh%2F3GSxXa%2Fi3jqo9%2B29iBoLUPHSkMDFtQtLiSrPa%2B8FbKec9UYu1NVHSLwGW5TCo4hs3adpoRr%2FavBVIAdFScc2MyzYymVn8onzpjhM42QVUILxVTX%2FGlTxuKm0c%2BWbW7ePuURqLIR%2FOmf%2Bog8YyJjOr&X-Amz-Signature=7c7caee9d01571cad7f23e43490368dcb5e2a6138747f5e49173f399cd7da719&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

