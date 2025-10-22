---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4QEBWQT%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T140116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIHQtVf94nv970bAyTl%2Bh3A8Voe0A4Y1SGs4DQFLB2xxQAiEAwT4xE981adUGRbTTsmweL%2FUDrMpG5Nt4gsEWCFZAtbkq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN6Rk62VPqAovQv9PyrcA7no%2BFjPpzQszJEliPg7e%2ByE1C5OJxgwak6tGChEkBzMVGOp1ZPQXr7cQ2D8gmDtl2QLRF4prEunmSMoQA7Zw1jqHjlgmYt3IeO51weX82up0pO405%2FvrRp%2BrhFNC78H5Z1%2F1uwiQQoHvX3GLCaVZf9h67YU7lLPt6gY7raEYa4gA6Nzd2gktrVO8GeJL8lz2Oda3EAUQsbCDjUs67tncDnr8zg4ybpZIOwEN%2BEmlajLvtx8YJsiV5QyMBxcQk1YhHLE0OmpkNKZbLrA1KF9XZByHXKVsELN0iqZoe5MUFCITU1vYZwVjd21zmFO7NffdbZswPK%2BuAeDEtdP3lkhHvalHIGkrAc%2BVB28tkoSVzrS4WPdHPFHDRqxjxgoFxZodilBUeCVNQGdMUYdtHpQud4badjW3%2FtlWBFSt8C6tntfGvC7mFuiSRfXVJhnYt1gWu04usGDCXGMPbBRMOkymtVeEwALGK7CSfQCwHhjsDtntORf%2BZ9kPJLXtUwoxsv51K6LDHOAWUx9ewV32PlWTBtmVASnSrPOowJZwHbp5Wbz5fhdUX%2Fqf%2FHOzlreo9rA%2FUS8ZXo%2FRodDwftXoFxAtSv5JvHB0dGBFEtZUQTvxvYv5tXTr98K4eJi2r%2BtMNCm48cGOqUBe17cak8Jg5jUbVOne54FhBA7Eff24foA8DYCm9o%2FcCR9MpchvvVYcgd0XGPUqKL4%2BM2YzJv5SUqaSOkDNoGBOq6XV7fOtMLS5JP2i9oUaSOnDVOfKZpWX6IEAnZqOhrjL%2Bg%2F04fThW9bDFCKd%2Fi5CbY5xjOZiDeEmknhjKFvqWYLvU9gdq7dfcmPGEdH8ldVRWiwFD3BE%2Bf1pVMnNbUHWRVJEdLr&X-Amz-Signature=320969fd3cf594a481a5610b31563e73c2e6423ef4096e04989635d0cad3be10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

