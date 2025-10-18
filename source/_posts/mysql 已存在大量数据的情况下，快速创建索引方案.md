---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSITP3OY%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIBNSlvk09gDKwJTWTIV1LcvyUsw4CNjlzc419nYYKXNrAiEAmVYBSWTKMUoLAbKvAmQAZuYbs3GS52GP0KJ4jLSQG0UqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHLZMB%2F%2Fj%2FLLYcanMSrcA7FXdNCTajlLyARQpIO5vTfI6eFmLC2wOcDfpCpuWZRKSaKsV14XAL63mTXYM%2FX5XGj3AgeFvIY%2B5W8qXFWRw9YvyWczS%2BTCFXQb0bCz9kyxzJrgShMxcIyAnpx0gSdIK0jvtIbQEWUrCoerzqA06JEdBMxWLgwFHmIU0zjSBuoHtVffLdYl6J2ntfl%2BBpKMd9MTuii73e1N%2BRLLdftgXMuSRrTsAaZ0aBZRggJu2e7%2BBNzm4dMjJcHkN3dv%2B5eGzGTKzVW7Mn6XZ4BpR9B710fD1r6s4xH1R9EvQA7WAYHzewd%2BIcUfjzhGdkHMvoCDpP87%2FAq9qdNfJuUIvIDEO8B5iLmxQTYa5KJIMABttIt5LetCcfqTgeESBYPg%2FTBg%2ByK2lbbFsHVTomjLYJklGl0T%2BdNx63BdRFzLFiuiZdiZ%2FoJlIw5X%2BgXSHiCc4iFgoRr6SNmEeUcx%2BSuBmU64GJpazBv%2FM%2FYgJ36JxnnnLLm9P64vTld8PFNJnUaSpiM2jg5P5ehOo945rGcS1bkAPRA%2BpyUubcy6xTQIXVr3Gbn0Spht1EkVcBtbgtSbpRO2oLydTN3Q0cBQ6liHlCmEX65Z%2FFgvzUzHdLAxF5V9%2BZ0wqTANx7KI%2FOsF7k7QMLSCzMcGOqUBT%2Bn0uOC7F7FbEXV3ob4TJAFx54XZ1ey0WzlyCLhhzqDdEfhJ%2FjUEcG83AlDHi2Ne%2F2ParijW5hwKppiQAFqLXSpHL05Dw%2Bm2JqbC8UIVX8NvEWTkEcrNEVRq36B1Vv0qSBcTkUfwuV1mfNwGcwn0iPcxJPp%2BUJV5VUJSCK%2BfYlhQkBFbDjIB77jC%2FKPpSjrc0CBKZ%2FzYpRf1rbOs3P88eupzF%2Bga&X-Amz-Signature=8cd15d405662ddf95c0f1b01c424946ef66654445d541f39d7750ad01889ab9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

