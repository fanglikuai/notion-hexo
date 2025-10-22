---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMXZC5WI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQCVTjAPSHLMxsnmnRESPmL3BjlbLPnr3gZicVmn6ZRVnQIhAP3K8K0eHb7GjKT7KjxuKdtxA%2BwgexOyOvm72pIKXVqGKv8DCCMQABoMNjM3NDIzMTgzODA1IgxgdobfKF4bPusJGwcq3AO7ONhKYHM6NHn%2FZskcijQ6hJLc0rWfPnRDSIYPzGwqXjwq%2FIT7JYAaiI1on31Fe0n4EU3wtnHaCI6MeIgP4WOs%2B2FwoR5xpxvLIweGK73h1EKmeclydniU8Lrvzvq1M%2B3ZOnLRPrHGvEcJyvRVMeV0EUAMpvXdI8BGoww2DvZU2qVAGh7Af66JsATOHFcilrzMwbF3RE6jugp6OOWWMX7BOaCJUoVDbXbFbT7ss5psx1Cxg8Fy3JSZTjICcFR6FXo1Ui2qTPpS95dC8m9lJTnB4oDKHI%2F6W6dWsfONZWkXuxdPP62Lonw9uLfdCuzGLlRzTsVA8Ge2FIkFHRmze12L1fqHexFNIvkFwILsA8Y3N%2BDjrRw3Hf8N9xBgrLKg8Kf8k5dA1ISunNfr7OWMnYD8oUgguKhJEONSXA9xLkq2%2BioKYG63cuo58aXFPVH6FkkgIMUjCzG34tsTCrjiScm2L1hXXJ7AldmmfLhJwEZ7htlYaPdZHc0NUC0UW4vzEsmslAp%2B3nPydZ2gfh3CXzrKUEAYXD5F79nBFHBncUoVeZunvfSfPKNYXShadlRtwmoQve5mx5e3hiOpvABSUoAY2Lm3cIg2F3E4e3FD48N9K3KpvrTPEF%2F%2FyUTpsDCl6eDHBjqkAbdnDUQ%2BpuR0Mmo0O%2FNB8Sf3Md7kaHBvfF6mi%2ByoiCrk1J2MBk5XE7lMiYb6VYscZI%2B2SH6P%2FIOojz5sp5fhaXGKWAnjigvC%2F1h8HJaWUiUSl22swgCgfCiQdhHD6bJEbgNmlalGO6JfBZGBsBfw9M6z5CfuIK9%2F7mHd3695SVgYyJncJaEhbH2Ofyy6MzQ%2BZqF6jj90DgTHEDVeLTED%2Bwqc7LB4&X-Amz-Signature=5550763435b20590e0395cb17d757862db68335d76fee07ad9cc61ea4ef0a647&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

