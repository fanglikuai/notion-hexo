---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCMVIJU%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDHkmGKYRIjN25oP6bQNkvioVBN2lUej0bIcNDOljbaLwIhANZgsiTtce%2FpuvAnAIJfy3FGAZmS1HpFOhTVGKE4DLtmKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgztL2jR798ZpLiLNgEq3AMcUsPYCWwpLJuPU0DFwCR0S%2F0MiIU35gzxnlE%2FsVAx%2BTR3ur15QnGwJ7nsEr55rmrX2LaaXcz%2Bq56%2Baf1ZQTPIztc6NOTow9NOBAuWRLBClcKh9%2Ff83N3yfTAN7Rm827H7G33CbNq77JKQob6WTBzmF9qP7RBFEj1aG1Z7w4UA10apAIHpjktNZHArX%2BqVkB48SFHpbLEDwY72Y5bC4yWk84edbKSropIeQU1Tt66g82FsY9vhdv3lKH5X5knmQ1b9d00Xbba6%2BB5iZ0xjdxZDUOBthdO1GFzQS%2BlVLFiCuHjlRDc1eTOs3d0eRARODfrIlksQGBwFa2KlbkSrJd3TrHow3%2BQo4oSOWtjoZmcm2gsaw9bAngHbTQhHC%2FGu%2Bh8OVNdRWjdrwUuwNMauYPZEuJJ%2FAAUjX4CDryOO32zYyF8Ow05dfY%2F87jDhDKkl0us2aZXfuFHoVxTVV%2BrcShOfhEZqGcjvkerWZyo3NzOKH679KnKESVuVwHUkWZB%2FJsfH8rXx8z5BAF3XMibSfh4EC%2FY13Vfj8f50XIPmlQKOhpljfqNMuRKnNgW%2BRIL8IcPGd3cR%2BKAE5lJg%2FGd7lxUnLFy7O7AoekDmF%2FdHFqhJ0qbMRj8fNul8Za6f1TC39oPIBjqkAQUUV%2B2uz1Kufms%2FPaOLyi6QqCYNRzBDhjQ47rskWd3RxvdZWrvsW3weLTnTltnD5SvJ3jzF5zRBIJcuWvYHTPk4iqGMAJxyqlHTJzxf5Ui5WxiEIJ8rNwcZ%2BDqr4v38NLWUDgW6aKU21xzYzsnHPtZAxBRCPyb5coKFuRKYB70NafLZRKw2lmeAaihmdWu5a5t49oMkaauTkZg8512tqUg2ZU78&X-Amz-Signature=76fc06707e6ff523ca8dbb37aa1f2650453c5217cb36414ca39022a2868a4275&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

