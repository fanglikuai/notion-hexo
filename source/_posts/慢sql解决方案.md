---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OR3FOPW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQDqZ4DFg49ql32yBXfMZBLb2ERkQdppgaTfQGVhSj%2FuwAIgNjeJWmrPAFJrOEIJ%2F675wXwO%2BBtE7rggaaHoqH1ajogq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDJ7E%2FaBC4h%2FB%2BRXIOyrcA378avRf5akymzWryEk31vj3kvbc8dCc%2BjS51qRN2TCvUDICaGFoc94Da9zFA%2BKe0jafvI%2FLsUZmGfC9tUGOHovxScKzejIXR9%2BLPQhRJ%2B1CHaZlXzF5HwV5EzZ1G01p%2FbSo4ezh7bOfNkoZES3uKZZ594T3wK%2Bi%2F36IMFsvHKZWWucogH0mJLUQkwhiGPjj%2BIdA4WrerfasxGMyYRHcbuDStITkfD8MmD19tlMO8IwF1SgsLQ%2F6If6gwnwquBFXXFkPmx1flOH62Bc85YczmtN2vXPZRXzv%2BISao3RH%2BoIC9vE9MtffGxmKp%2BsSd%2FG64Qb3bCES31ZrDPyKb4pfr6D59Jyw%2FYlcr5%2BU4m5xWljNrfOEDwgm1k%2BKUP9MHNJeC%2FhNoLEF68hEVBoptHg0rCI3ym%2FWZcw%2FUAiMgOm9N1lWidkY9M2VWl9au%2FpI0q0O1jdMV9GPktSRMFhmecqO%2BLcUg9I8DtR8oMsulfYfpydnozYu26YI2BvbH7bds6COO0r10hYh%2F5aAAxmCvvX%2FTmKZS94ZhflwmnbwzlDSgrwhvp7SqogsD2nPXiJUoN7NUozmfvVt45IeXzbshJ7OWB0ljrNc04AFrcxMcpe8VVFMCfA%2FnBvBYBJcBH9iMITpzsgGOqUBx4%2BFGIk4%2FtKj7RVEMHHjJtNGDokjIVFmKM2M%2FFe38Df80XyVlasP8v7f1P%2BMEOAEzZLY47PTh7CqD1kxcCVCfCEWFf%2Br%2B7oh3%2BAlQ6Y6wRATZSOiR%2FarIkuZ%2BrwERwKfZiKp7%2Fq9VvzVt%2BVnE97fHyPA7XbcKu4doVWtBCJLH3Jc1hYC6t%2Br9Joc3%2FsZCX1BTIp%2B0m1EJ9x5g%2FheKG0YLOZRvAJ0&X-Amz-Signature=38b8c7c7f5d2975a13f09e3627da14d48272f5b82100c436bf85565e36af8e66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

