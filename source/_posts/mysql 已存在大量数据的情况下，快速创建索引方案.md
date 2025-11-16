---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JY73VBY%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtxX64Jp3bSSCRinITTKrj47L0zAGI%2Bh%2FmH8YR31AQ5gIgaob7m55XQklpTXIQ7mnQ2xuXNwUom1ygMbOVMfOhlgAqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDEyTsqhmJmSlLNvVyrcA0EZOrYqxCLZ2kqzJsHbF4qYhtG0MJo%2Fnc25t%2BpEm%2B8qdj7jSXrW0cQhuOcvEpWyrwSH9CXDlcBEOuu4Ld2AVkmNFab1kXAi3Wfzpk5JYL41bLVa8s%2BV7%2BSp2vppCj6ThY%2BOFrLMhZogANzNGA1De6di1OwmHzLCS%2FEX4TC03ensXozFcWQwjlAAhnG7ojpRcTFSoYwi%2BGrNHRULLYzcaH%2B%2B5Y5RECnjD0M0W2Csrc9d1JIIQpNlLnyh2afAxgHvg0SF3PE1fYIJF5nldnm52NWNA%2FyC0%2BlRQG3CFLT65IzkEp8w3mgbwzZWrKLS1ti5iL7m7iO8E%2FnTRkKM9ic0phYMzxBZ094ZO%2BxmhQe8EajeoQDzOUcJJ%2B211sPL2Sz1IepcgBYHu2BDkI8sOZH0KduD8TFOyuKixAHzDamH13d4WGg6gEmzMwamRIOhjHvrEmnP3t8FfBk90x6eBcnBQ6l4w73dicDtT%2Fnr49Y96MrYUjm7Uyo8Lq0uAZ44UG%2Be9ZIouhW9cqNE0OktxTUVrZ5aqdwSzeRKNukXA83aCnrx51Kigt5rMbwPsnhwOhrdvzpRVBhTjt2WwyGTyQU016p7ThLo8WU0IzsV79RiZlvABjhSsOcO4vZM%2BZ2MMKj%2B5cgGOqUBT2uZhBFImP6C2HfHz03hLtaHm2l2BMEY502gPTlBzhAvMBBZ5opRR%2BWRK76jrjf9GCWrHajEYsGJLeLwtsOCSbamiFnEV1LfpKNWpsI4YH1mzGvUewJWEXX07Jo6xiCMeEfDnnFRhM7ToQj9QKvhX3GY9ES30zw77sZ0JmK%2B2ZdNfJK4NBVQYuclAhr5lITYlsmbyN4kzPLUFnSejpEWsy4cQ4KU&X-Amz-Signature=e7587670240805b02e8eeb0265d1747c48f943e4bbd48dbecd04d22ac6d48668&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

