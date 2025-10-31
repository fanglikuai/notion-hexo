---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TON7P7LQ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIQDhNcG4J6Je%2FXJi5lcXTIhaK6uh%2Fxy25E6Dcx%2BGErJXdQIgDzIIlPw7VryKf%2FMm8wvjv31DMwceqDec2Y6UFqkkOFkq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDP6d4BIhGw3vXvIKTyrcA5JU8B8t8Mf7yUb1M2SPlPsGQv2kMRWiOZnzESHTsV8LpR6S%2FOgzWcoGuixbDF%2BCudkjwTmUu2ozBS3DmZdxZ%2FbrjOAfzj%2BgsYrTGOCUka9Ltc256T5fwndWAvclkuKHxAMjotcaNJuw4a5C2udrJkXJrGESOrwLpmKrloEQfB1WQHLOiSXn1JccsZVG5uGIpJjxB%2FW87Tw6dv3Ex0AOrOcIoUvmF%2BWrXRtegrfWO4RoHlrFCXadR8X3YUNuJ%2BsdGU8NvAS9kHmJyFB7Rb6JdMkX%2F3hhlf0NvrR1Qk%2BL5OvL5UgWnk7pZAQmsT7y4CHWAtoMTufyAJcQpvveZTtX2JNkW6E1ND4irTI2NuIZvO7zMh0omQI0qQSSODklnR183iTdwPe0Al8aO4n71M21tFJJckcGY6n4Nlf6RTggB4IHcKbaUi1o6MJFWRH65TwthoxZgF8fWvj3Vfd5ZdDoNRvC6U3p2487DLv%2FgXHQUw2D7vJTJ9HigNcymDd8l7Jqm8HkxiEQMMvPj8M1IYAXQDVz9GkodayarOIH4CY7rRYIAHrjBwSzoxa4okMSc%2FQAPTTq8N595ujCXO6%2FWsB98q7MAymyaUXzjc2eLR3FtKfuDTa18e24B%2Fnnqb6%2BMLbck8gGOqUBg0Xbze5gCfdbwHjfuLF9x3C4zxBM%2F%2B15gyMLp%2BLdP7Wzot3s84LITmI5piyGaokKLgy1dtv9I8yd5E6gGFO9AZhKt3C3PbTg6Hsyo41QtzGQ5aycdhlN2byrIdyZZyCKZhG5vHDjLhtb3mXH7QpX3g%2BJVEQBPp%2FClBzQdCh1%2FdJyD%2BpNWQAyuPmm3ETtL9h5cXQckS0JpdkNLtLDMp%2Fp7cNJTxrO&X-Amz-Signature=6182e72959b6f0f20ae0de72fcce86977a30418bbeb560a755d107370ae17edd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

