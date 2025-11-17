---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667353DXDL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCVH2pmgatYQKA8Ts6EGMaY8XrvbSpz3NPIBTTaHt%2BLaAIhAMbyCKeKcU8vKLQKYZgUZazoNyeGEUK46Rp9Hg366ulUKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzpgE5osWrXXvwloPUq3AO6r9%2Bq1OCYYLT3TWxj5GSjxDfonCDHZGKb3hyvOE7hfcaFITD6vlqgBenC7BuvOQ0ziJ8RhI%2BdQb0z%2BBf%2FFUb8aUnPI%2Fd57jchzWG3kmMyD4%2B0pORmkXlZuS9ok3ymioYiRRc1luYZm7CAqfFtSEDL6nDdeSkejC%2B%2BPefGaUCBIsrE4%2BUZzzdgp4W3p4D9g33by4XRJ4QbUn6%2BplQaVGGIrMjcs07E218tvdM4eiVUT5Pd6sVOnv2xGSHf7fpM20ylrJUB%2FlGj%2Ft6Vvu7DQ6jnHy1ZI6ShAZ%2F%2FTB0qAzSozj5bl9Xn%2B6XSz%2B0eiXqcPc4q7H9TnVcgKzPAWKbtXWBbMUv5Zw2tjgf1g8sqFWS5%2BS%2B2frruLApBHLxsKYXkB4IXxD%2BMTDOov3nQibw1IyIjLUE4lwsz9BMwVJvqoKAKy4C%2BbyeluCL02zRCwf3xQphq2tlKpyNHoCKEFEPqI8n09GWT123xbKORpsePQqc4zrBFGvkI7Fkk%2BZdiA3e4ZOmMMkvuW7rWkIwv3CYCC1R66YdzXnYtivSs1m6khS7ulJuW4kPLLWLai87SGqKzXQEwfSNJAOnkam7jId1fzlINlxU8IPFYF7PI3gUXMBjUtd3GsYrlkugv9h9mwjDJnu7IBjqkAep1t%2FNeoNsETp6xTNWA5KMqvZaYmf5uAungh70VNmEON%2FLP14Xtkz0NzuvZKifX9sk6kFqUkoaki%2FcyB4YUjT86ki3akCI8QaUDdJ1fWN6Ax6uCRuJhq4b%2BJke0rpa6Nadl%2FWLTf%2FTnmlD40GYbXWMgCEx94M1sqV2Zokqg4sEpKYu6efJU%2FYxxnUeO9qNeW%2B98vroT0CQnktOF%2FUZnaXs9hW4F&X-Amz-Signature=4257d34f236fd94d7cc72eee7e023a51ff6bdb1a1e36ffa93a8a56b29f50319d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

