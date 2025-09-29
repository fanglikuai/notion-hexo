---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZAOOVG2%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJIMEYCIQCG5lLmMBIVzdDLFEiV1VTXTGt87%2B9knWgUYcHhOGeUnAIhAJepOpvUBudsZ5ygf%2FLyupGYSCBa8cD3BXUPlzncze%2B2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw6l0aQn7rSXx43fJMq3AMoHlFe0Bw7bVdsnBIvBamCkbMJo3oKMFRuZj1sa1lPok99zWox2Kk7U0zgRmc6U27mLWRFxvbWLyjBAmE8opFQhXpHeUSFXtUrdJj0XHvEHDr28XHI4a0SxRArmwTslOeYT%2F6f%2BVZhv%2BDfYGDxeXhfy1DuZ5Z6gdkUau4jpdzQ8Mrb2GOGQNY%2F6e0vXCWLIH%2Fvsort1mNFPJ2O71z9JMN9918Cy7hFU%2FAlUY09SPavI9jpvCV3PG8ZgHx2HiPLRJUWGLWv1NNho58kTNzPWj9wMLxQKFG6W%2FzLaXCzA%2F1AwtNBw9AHZU98oueMyUHT7oGDW4Nce7LnQbGJRxMDtSqMvVWLBf1oc7tLzWUtY3ajBg4fir5EfgMzM3Uyy538fX1OkyyO9D1wSgYLjvbP0TnDCWjsfwdnHwjSQo7TheVhnqNchKRGTUGxRss7wROZjRZybkQgDe9oyoN65Ym7OuRYtycQLcdfVnvageoQK%2BMw%2BHM%2F5%2FDejdnz9svllxtSb2wG1eU4LySJx8sGag326LfPRg1N3gdvVeVNZxUraZa4SqIOTmSVeOYSlwfH9XoKhykiEXdW20VZPqhSFxuoTfXXXGqK6mZZAOV8QUPHiP%2BSZWk5Lk%2FKzyUxGAY%2FRTChgufGBjqkAcAjxtT9JrTZMZwSWzL7vcrsKEru5YWe9UmNSNrayje3CzgcwaarVHYaJWQxUiP4PRuIo4UGUVf1W%2Fy1pE7JTO1ruLFva0hROik7IOzJcMBYot8PdtUEqPXdCY77Vqo85vFwEhhBwY3PmC0gSokf187s8SCfwzw4YDUcUroc%2Bkd%2BxF%2Be5ndafg%2FhQKd1NATnWo8GdpPjhez9ALehGMEUyTZO7tn1&X-Amz-Signature=e8e15f74d2847a864520d9899b6d8d038a0471a1ac47588a4518209b8382de73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

