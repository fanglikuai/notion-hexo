---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USCVHRXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqNgFDcoXQYATk7HlzSsUmUjU4DQXgHwQvtpANY3j9dAiBwAmoFfFxBS8gnskjtkeSm5yHG5RZqRhJqOxr2CWXLSyr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIM28n%2BC7j8%2BeVj7BL%2BKtwDi6waA%2FEeVLOEHvzjKXthIkNjWqzuEmNQiJXH%2FPc%2BP5FrluHwvrNBV4Qy4n8qggH1bN1iXRVgjvmr%2FIAkoMXo2A9b1FOVJQZ13T1DP509559j3XKJM%2ByQGiml02TJjihDWBOOn69XieJv%2FtoMtFY0LDRLC92hV0XODna2ZkH1a7wpcaW4h0RRR6KsybnREIg1yu%2BFu%2FGPsbJieBR77TeiQr%2BCi9U%2BCCxkEqawk5GPetlF2aQQIAaTO0Y6G9eDkKzh96RfYtwJp7S%2F%2FCVVmbJmZGqdnU%2FqZPn4GyQ%2BLGRnrHH6Cb8hQa4yaKxAJ%2BMD%2Ffm4AAOlVsf19JkcjyUrLUEh%2Bh37rU3xJqs0meCFSPCJPs6DX8dF14BmSgexIhu9fcd5Gs19kFYWFzSBPbf1Bw8w%2B4MMlNyRHm1ujMw6oiwtnUrmCIKvtRYzOVRkzp6yX6Dihb37Xfl1gEFfF0hAcTBBXrFiDQQqO9C%2BGH8916sOhnCM6HLA%2BwrzRbl9K7sFe%2BJPAskI1Phcw%2BMVQ9xvItm4LkhqOLy1ELTF%2Bu4uVqGcgR4W0q4BpyBZs%2B3B5nF8jaxx6RVHfW8evcSXl0vzj6yzXXkFYqa4dm7eUtfSwXwwhjRhKOoVIl%2Bwu1G8XYww36HXyAY6pgF43WJ81dPhdm9AyRO5YTlp26rXBXNWJ1DBC7P9W9jByEWv1Gdo8vxI2K0rsAQg0clweUj6qUBYVoE8qQzq5rJr2iMRUtiGarkP%2BhTYckaiL8ko6WUx%2B4UO5GFaKJsvYvb1OXv6WtdKjVZ7O7TWsCfAVbxP2ECSQOi%2Bq2B8GUiXNSr2C0BjVMu0bEIGw67tO7RpioQJKQi5pJjomW8ZTOzAdrlWOhBI&X-Amz-Signature=19e90fe2af4df36fd4a51fa4a31983907e694d7257e25f6982c7ae0a2e0715e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

