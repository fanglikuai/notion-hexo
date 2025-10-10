---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RXROGNR%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIBUYjZ8r%2B2Vjo%2FHMPY%2BQTYk7S8aOF3VkPiVdzrMpVdkLAiEA19F2w%2Ftm6SSn6f%2FnOXnvImGZT9GyEPB2OQptLfiZhsUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLJ%2F7AnpgA0TO3PJvircAxMkV6%2B6%2FHkqkG%2BzAUQZ5bpzpoIqBNAgQp1oKwiHubXRwFUzISrcytosi%2BwMFe96I7dNDyw0RSPUchsmApOEMG88LdBSJ2WZxyhR6NdKgPb9HhzIasCjfHfvbeNeSMLBxpWSiZL%2Fjn9Ze1RUcnkXJX3g0bAn%2BmBqXCQfHFVuwdnEeukF7QtzJXjmkAh4iuquz%2BAPzS0JRUKzazp%2FhNxMYwsU1W5vLnYHY1Zre3tVsezQzNRTXWFckC0tyNXHVXWGQoeFchNIgGrSc5IilquRtG94fa%2BZZGkQA9%2Ffg8FTY11B0g1cC0kKb8foreLVoOtb9jTTA0RsSIeI41tViiuiuRvJsywa%2Bb3RFzeOWDNporiMxrlsn7nEpvKp01kqzG6tJNQHCLbMB4wzPc4Q%2F4piJGbz1nFwym8mR%2BOyaucsXagy%2BR39dtJa6FyeNY7i0F3IVlVGFvIPrJE%2BDu2H0XS6L9T31Bv9yB%2BN%2Fx9LFLXSKjAaTn7gE6PEA9lAWB0q%2FepN4VmmGwOHQCTMZ0bu7PXsv3dKcRDLd6cRrw2Lv%2F6tMva06rBg0OYlTjecPfGuGyJXeLkTahTyD9T0X56NA4%2BFZDkW%2FU7FAWwsqMojkRos0%2F%2BcFtxpFc1JJvNnWKpkMNnZpMcGOqUBDqqHxRckJkN%2F%2FrafI7IJnzQxlSTmhgnD1Fi5neYoTwRXG0U58Up9VBupDt0vZdh549bLs%2F0biwd%2FF6VwkBe%2FqShJDkanrVuxYU1iM9e5JFoCArUy3hfFbvR1EFFkqpeCyqaSIKYKzAORS6kxjKXPS5C2jPkxsqVPRqgqiUbY7VFpHZLWDMZjKCwD6mrD6G0GXZA2Fqw9vqBI5QvbnB2MLNa6X%2F7f&X-Amz-Signature=b55adf242beae308f48b221150bfbfd09756fdd7581248901bee609fe7afa9f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

