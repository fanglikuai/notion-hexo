---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665WR2PYPL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCaG6rhKalIeR5D05uXLzgwu1pb9xgXZYIEHYktzWpy8QIgDBW6UTwXL3xFc00b7b7xtyQrRxzPC%2F5vBrCv6v3IbDwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGu8SdlTL%2BX8DDeanSrcA8YRm6CRGr18BQgF%2FlkFgwxT3ImXCUSQ1bCgwyA7w1OzlM7Xcx5LA9EEprdee5yKxweju0BMnNKenvsWAx8wzVofm%2BmR2DP4H0NpTw0bJnqZNENz5z8U%2BrfCv%2BPRZsnAag2ZqG1xCa4Pw3BpE3ecEIajPzWpCvLI8M97DwlvDZgxmGm2%2F4%2Bb1n3qUfuMPEUjgg9K2e8jbP5WL%2FPquaT1A7ixjWBK0M1PF67b9DD4ugaXe6LzOpzHHIXO6NvY4LfnGFNXt146ahwB73F7%2FVByD5cHONmsihbTkBs%2BGgMOUmLqikW7MngxzgbcM%2BzGUvzoDvy9CBfK4i0BZkGtRzQfjwKK0KlV8Ag4ugaWJF%2Bs6H%2FkjTRsS5VyXY9tTxXmhwhXSI9JFrdht6cd7btqcFwLjFWjHRuCnfLe4M5vJjczjOsLg970N3jXiZw%2Ba6DZ3U1VqkaTIK3BrQM9TeDakoRXyCSqusn1mesqj1A1aqdOBcYZtGN%2FZKA%2Fs8NNn5V6KV2xgvq3W51cHfuQmA2uqRUTeZsaw9BOnhaf2gl91fisrEPKHP1t7rR%2BHYrEMj2yl8q%2F8aaFppMcxKWSXUivsvXlQRaMvLMEhw7VIwcmscabfhmIDZ3G7UxIbHjO3ULJMNXhlMgGOqUBc41NwUyCSVh6W%2BBc%2BnmbXYDtvxLWjxKXD6nukm1UgPwzdYGeuWcVlV0fSgqHR%2BbTUwo9HucTdMd7VvI6wr58ngXEHRnIYc29PdZcxCU8YDXIQ1D1Sus5hs46wRuFW6ptKpwmseiY3JNlpPPJcNLSIlwBOVaxO9DMFZ7Wf5hI6A7Zb8g0i6u6TslSjUSq%2Bg%2F4xK9vVqA0drrwUXfvGVrq%2FshQRbjD&X-Amz-Signature=7ab625ccb88d0bc51b4d862d7d5684d902d8afb005773269a7de55f5233add03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

