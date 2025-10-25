---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U4GCPUB%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaOQmvEyFFgeX2U7xLDsOqgBWRLdL9xEFXbNYUxOnVsgIgdLktrfJNk1hakTDuI%2F1ruj8v3cdOZ4dcIb7wd6aj4Hkq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDCl9o%2BUQl%2BXRVy3pKSrcAwLE%2Fw%2BU61jTQ61EPSPpj%2BUy4tbAc%2B4Mfnbc7DewQqWg9b08jYTk99xki4Slx1mQ1ojX2TPoILuosezGXNCqZVAVcvxuheTjWAoHm3yB4GjVk1EJL1OnH028TDBpjIxyQaGvoQriGQXiZz2vicZRW6OD6pKQEKU3SHBIF%2B4AC8Jov8mM96alV4eg4SkAFygtcwlzliL1RCw8ktBcZK6cwUu4LZPORi9f9PSEr0lMkP00LMCt25qExaE71F%2FfqTc87vLVNYj4hslqQ6OKPS1uy6tDQf6mSwkYMKR8mMroV6x%2BoJdYzZ01kORFRY33rrnjo%2FTXcyMCjq94cOPEQFLUXnAGKvGkwSu0Ax9ImSlA80dc9FMDGggM%2F6%2BSBCvO9%2Fcv8y%2FvOryfKHOlRVDQofW6HGj9%2FN4o737FHvDXSfzhTdSieUQdiWmrLjcDBGm%2B9z%2BywDR%2FiGoO19r1x2vrDcFZG3%2FCLK99HgDdDhgkjRb2t5H6mI9cWJkLgjDcNbANi%2BnCqXU78DIEPdK9NIb9WpqM9JeajTxW8hTMZl4%2FspjCp5g%2FR78aSLXTKHWYU9TD6k5eaCXgknEMmIln0mzUTNzFxJlOMA%2BEg4LEH3UCA%2B3m76N9RKITARSpx8WzDYq8MLfq8ccGOqUBlGVvXhC2%2FgOxwJFZkKXMVM2osj1943HiTC%2BJ29DVJkIsGAUZhA1Fz9TRSGuhXuwhXDUAvI840aqfMaOEyi1AQu%2F8ymvKzX7%2BrwMGQKpR%2BvapCYl8aSeKzDjd1cIb5sKCUCf8iQ3lbmHXjim5FQwDoo7XFCoy%2FQdC2YZLUHKwy0K1NBenH960Eejm57Ux6ZPA5gd%2FfftDnklLDBLtMLgNUtguTA4D&X-Amz-Signature=53e5da18bd391e5bb5873bd23340bbc0198f674a96cf049990dbdb45e5eb209a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

