---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQTZTIQ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDk2aTBSTAmECutTU4Cw%2BhJ5ITOoKILCgrZovIK6lC7SAiEAtg6tWSly8HfKCTPqqs%2BJag6K7tQx0f8joteUQcBGbhkq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDJsXHjZ%2B2Zyq6u20IyrcAy3VV8RMR0J7f2gRZjAwHaKjt%2BwNu7IZRwtEUZt3Nwqi5ElFLf48kA5aMIr1UV79oynh053lhryFOMHPrAjkSTkAX4hOm0bPJ6wOQJjRM0aE6BmdNn4L6qNSIz%2BMdlkjwuqJ3iq2jnThXzW1DBptvJGLuXZsTXhPqUBD9sh1nllrcJ30D9YOJ6aZZCSxWJL0uU6L7ifooLERxyrlR7wor44dyg1DOqSbXzFnmnCgDbuAPtHabhhB%2Bw7wndhXB7bTXco8NHlYJ1x8maRXgOn4rE4o6kzFH9Q94URbeb%2F%2BBSljlHhirfQ9%2FiVe4jBtS9PLs5%2F3gJBFYJwYJ%2Bdken1nJ%2BcPDkPD%2F7JilMoL1YzWKOsvKRCejUD1NV70wbziuuEZaBUaC%2FFJ2XgSVS7P9OHgE0U2FF6OJw3QAh%2Baxj5lWvskr38kG33yP3z4shBXZE7eI9x9lb6neJAHD6%2F9L8I6rG6uqtxEGeTLxsTB60E1y1VCyhpUuzMjEySj0tWuSiHQQn%2FJ6CgvWjQGDIqfS9E4bVqtjyeK72CAR8yTLvy16D4vC%2FzrxlXrZm8D6KewOsuVK%2BylhfiwmxwlJTL9hKyPOneu5Nw8iyvNlGJuqEtqQIz9cirNDRvdKTmLIaJZMOCtx8YGOqUBIz5fEcnlSzBUR8GeHVeeQlqL0vKadIWJZMnTkuY2M5%2FGxTHPn1TFttqrqZT0%2BqoJ2zZFRTr1ZTYgruBvwSOPrRtGqykbwRIMTrctU5pXEDMeCqxmLZrt6SxnulqzKPjUcRQjSCaM8OUqrdg5TE0SVRXShj0DO9%2F%2B476jx5bzXgxA0ynPJV0wObAgrylg89NgIHt%2BLkHicvDTLba7A0pmWPXIQbgu&X-Amz-Signature=e2c41ac245f6136188aa3b834a1ff61e0e3e00b601fdc10245490e3d93179b47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

