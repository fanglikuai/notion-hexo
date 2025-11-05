---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJWA2AYD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T150104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAQ8747xHI31sSyeaeF6Is1vOSjJWFnDNU2hDK1NDKiEAiBkhxyZF4FcBPj3LKW866NeaC%2BgtInYBjWMR4AH1957VCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtjYGQQnza2ksIFlUKtwDg7eASpUnrDQz%2B82Bu%2B2JkhxqS%2F4wBH%2FpF3kAjkcmBtAF%2FqjJmDRa6TC0QiWEm%2FnFEQef3mEWF1rQ3Jk8Ae1%2FqhdoEK1ow0JDqzVsaENQsd1coRurBLCjCTUw6oFidnKglrN7Gq%2B%2FU9PMbWE3sYNzIJB1SLZU68HNR7HEtul1R9JyeZ9lWkvVs%2F%2FkmDvgFec7skj4lb6VlzEMkWaNJrUoJC7VGBgVDwejYq1ytdwhxTN54miN2SNwZaYDQSVM8I7aYpHDgskUDiNIj657nkVfEVheROAYttVY6whyzUHV9FOi76zf4ncRsjK2zBEAj0jj4Q2pNpaoqPxzg%2BUUBZXIIXrtjMvo%2FuTwrxi8El8TbEdQ7jRtRpef%2FXwscqtbM1kRbTwQ%2BP72JAdVMwBEjQvy4U790TMmgaY2FQOubCPmOseH0stuqf2K3nB%2F9iHASPkTDNo%2FkeLc00hIjjhdhFHVstFO8DkP1mvzZ74TOZIIpnTDOyFDHywI4qHN%2FNGbHnnRFskzrFxBoWzFuwJD9nrzHIu2yjj7qEgniW2J6KVeVwcnn4IOkrH9vU43ARHJzRwvkU9x%2BjYopntgXwC%2BFhUc%2FwJ7WKwMaojAC2yuz2SkMS9lcl2gT7E1hh7qcm4wsrqtyAY6pgHtfmfGMw4QCxnXyAEircUyziNRsFko84jLtFJG1MDK05fPHZCSe0EqcI0c8RwQppIQ2I4Bu%2BB%2BDfZXpStYqKZl6xxSSK2BE1Ir5Rd9HpnyBLvrn8P3K5Io51l6mWnV1iH11nGGokjL0rQ8CJAvKF2UPc5P8Cj3JExSBCAkzBhQiOznkgXC0otWe0CQ2rsE7DFyE3IEd9omkvA6sdTHwQqMaTjDEj60&X-Amz-Signature=54c05750b26ae165b34c36cf80da6daa71905f00e268ee14f443e147ad45a0ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

