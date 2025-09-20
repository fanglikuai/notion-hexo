---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJT5V4X6%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIDSDX8Qx0NfGlxRU9l%2FYdnHy6LJgZN5gS%2FkgT6tnDpPeAiEAno5iVC5DJhmjodEMR4tJo%2Bo8OLqdZMJl8ijLpfi61K4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNP92QMvBwKAmzpVsyrcA2kRXNuzINvsERmsmwpZwW1o34vHqjnhv3J5rNbLQsKX%2FPiAGSug3ZZsIWoLcakkh%2B6nxn1SMGevZyvDEELzvvQtQMSkyeBHqk4RMAtPaK1k3dY%2FTikGcQp5wJyXwf3VoR6NUWQbuJwGMYTW2CrLGuup5%2F2YoX6LBHLb9x5rMfmCs%2F5HvURaO3TQlVEr7xUaSEPfmitPT%2FKs5WP3VE4CwQ7eH3MPYDCIcKFgzEki%2Fm2Vz98XJGkxJhH56lI9bJ3kzBe3ARNs6ipRKSbdxrMk0oOpBtx70%2FzE%2F2DrU9WqmfC92N8jVa9CUCL5j6Tpme6BeQOYvpMfZN90y08QopKbd6GgtrOPIfQ5%2BewVieeDDN3MSs7%2FgKvjbp0uu2y99OGHvZLHCxTjOMW3QIGi4zODLu%2Ftkll%2BvOrgOf2o%2FlE9ZRMpYlKRR7Yh1IbiGzs259ihPI5P3w1tnSOnJ33ww%2FxNkkFtlQBtnCgLT%2FirkSF92Qkipv2c7By1e6c7Z6Y64g85irVbYCn2ot4%2Fu6xDjVvPPEDJPNOPTlrbbHpKjRrBLv3cq8Aoti6jAOHYCPrGUP8%2F%2BJNFo5TVjDXftmVEz31YR1fJBGJUmE1Q41EbmVCROpEJQjR%2FNnK5aXnm0DG5MIXouMYGOqUB1Lu1PRXX%2FPXeCEKLV94G7SprnmQBq5lJyy3PAtkGXsaRIfMtfoZhws1LaT9upvKGkj4v5QVVofYPwPq4xPutvtM7rCyZC7TMmJQDGeXVDSrvFHWblHpsnwkjKWNrwXPMKv0YcRAL%2FFZYUTaEjkSWDhO950%2FdeYkAv3fiGVeMWj0g82LqTwtbHX7m6loEeIDVDWw4crQFKLm4%2B%2BGkryCJS%2FsX7Tve&X-Amz-Signature=7229a34ab4c0c9a2baf4baf51a1932b1a1e9de5de84aa053b00cc98e1818c66b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

