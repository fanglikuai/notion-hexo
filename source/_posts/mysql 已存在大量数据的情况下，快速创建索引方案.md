---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOMH4S7B%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQCAO7NN%2B7FD4z0ud14Wrpn%2FA7od2EaYxmkJv1NJ9aCKigIhAPWJQceeSc%2Bs8eBUVhl7IQNaKJfnkRMPHnPgdeGg%2BC%2FfKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxL7O0HNJ285oAq5aQq3AMuWZOHONKPdgp6Q%2Bb1%2BaYqOftjUxkQMg5K1is%2BbeXwIejas%2FGzid6dSzQlnTMTuPBXZw%2FJchK%2FF60hc%2BpSdPcXGp2lIDO8AAhe%2FtNKldhCWHeGx0I35pWEGuVuBtuIa4U0O%2BNKYD9WKplk8EnheTxyQe6YLcvJuT8162gTRPZmQT4kq6U%2FyvOuJMXsNdMU5A9KXGcVmnkD1bpbCNYRUMxUZ0hXcgjNO677OpFLmwYs8DweJphpt06wu3uP9Uwq4bX11CuAbATgNfwSqZUX6yeYKmurSDWxi%2BmGYrN4VnP%2BQ4EMtpoDlghMIFTyxzdozSFdNBus2N8JM%2BZ%2B%2Bh2q%2BizcSUAFBarrcCck4M0VCui34c%2FRwnmEVK2a%2FTQSTMNJe0kpouc4EEPDigGqc8%2BxC5b8Hz%2FmTHmBaYXlMpyL4VAYDvB%2BGs6gYPecxmWQnywg%2BnOTtXhPIXarrPT6h4%2F9EMt2F3%2BoDbrlOStVqo1YY9OfTXt8mOUoXMLzgfuGJ%2BJKCfcvXG0Bnygz%2FyoOgrXcA9BRildXFOw2g6PwkC0pNqkjGMsp35NpqXYF%2BPFjCL0weade%2FjK%2BzWUA3h0ve0Wyo%2Bdy6CsIk4XNzp%2FQRCKNa0T3zYOmopKk2cAgaQVkBjDNoJXHBjqkARJL2WaWuLPohZnNLMLr8AW09SN2W1%2F2M7prGw4nnA%2Fvt2aERPd7eBdKYg3LKz%2FqLcVtnPrvk0jrdHLnSY%2FvXEnu9WitZJXSeM4%2B%2Fgqoy%2FKd7JW9nXVvjiNMtHp5tM56mPRSltuIeubupNEyqhcAz7xJwR3CzOkGaJ5awwlmSMRSNt30cCOrsIJVzNVPYMnO%2FwCahk3hE4t4JEm3flgIAmctfeCq&X-Amz-Signature=f0b142f0a9738f78108456050bfad1c947c88caafbfde8ff619a21dd9bdb4267&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

