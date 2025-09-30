---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMMED5KY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIEwAn%2FMvuSNQzfovrFB%2BzzzWhyNPMsq1fsLM6YiCdY5%2BAiEAqhQfzKGFcRdC1CQ2h3M6h1QmWSHBPlpdlqFf1Z1TbtwqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQ7LSuKREgcaDPpMSrcA0Xffcppaat9xQryPiccR4RlspV2n7UDGcRbqjS1pC%2BXAFC8BWjLU7TVtKVPsyAw7VsEkb3AKxzUdZrEKuVNd6VKQL%2B80oRnjrBQ4vyLbjOCQ1RHnIArysJkI%2BSG9S3kL8oOD9aScTdZENM6kDvHfSKRZ87VYPvYngEf%2FBUinkOLpiwmeKhPB5EslnrkZudK%2BP5Q2wzEpS8FTtUw4EC7OcKACpfgFUS4ji2obsTy8P9jLo1EUCUr0apifg2N2hKEZKFz2t4ScpMF8zpaMtNs8Ra%2Bqk8EIKUA1LDmndgbg%2BERDhlFWrHTNvZiW915qDk0kbVq2twvaRWHmfR0D67ikx2GcWOWeRQYR0xuuVa%2Bx1puD%2FqlY5csaoooqQWK58o7d1I7jjw8mQhMNormaGBizbpeFRvSPJPRJD8ZRMhipnp%2BCw5LQDRot6l080Yf1xlJcEx4ZE%2F4YsRQcGyRdi2TY0Fqq6obj%2BghplKjuLMpf29EtIorBdnN93VbQctjAsEI4529up%2Fuq9JC0SmKUVM0YAzWE%2Fg8mdhMqe5IC1oKeyCQibj71TzqEECAfbJSG3KFj%2F4EWTt0rE7eK1Kv1okwexQ%2FWAgP%2BaKswxe%2FVEE3N4NTLvtOqOPAYY44lyKZMKKX8MYGOqUBiXhIVoNTstAZUGmD37V64lVasAUOdQ2rzPLhkw3039Zw6L9shicgjBwLbqleAKqtZBYBLWzXNQXk2n8ad6FVgEjiC9kvM2MYzO83t%2BtBC8BI%2FrngSEdHfbsTDRkMPJDAZ1RROkcCe8PTd%2FKR9gTofJ9kR2bqWrNkvk4neavp8tffzjh0ZlIUK2Rs15qoT1uS0c36lHiVzEAa0nWYmPi95W9C0hKB&X-Amz-Signature=e974954f95ae010d2012c80514d2f818b9a9edca6d40b0e9d730a3860959d523&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

