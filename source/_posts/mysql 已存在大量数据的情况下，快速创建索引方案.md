---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YH3NQLYG%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQDdyNXOnL7ZmnA%2Fo2wXKl%2Fz1QLJFzmSMVAFdYVT0PTQiwIhAKg08bQg7yxt39EglLoA1m7YSlw0pWnCkaQzoVhGuKilKv8DCDYQABoMNjM3NDIzMTgzODA1Igyf8PBmDu%2BNjhnRAvMq3AMpnQYgAwDcJZ0W9NiZ8UPnawlJsZa49fS7bMJcAgQSna%2FxgynaK%2FopQcjy7a4Ng24hbaaoTj4a5YUUAPmsyNfMKC3l0LHrtbyMdFWbh5%2FgJF%2BSVE4U0NxBq9UfoJBoPVt4KCrp3Ndf9EfNqhlCn%2BBO7Stv3YeWZWplwJw7NsyMDRlzszB%2BDctFXjU7pif75uRn%2BbEiZ0B2om3Cxld1V0Pe2jIlS70RhhbK5S1ePCYrN7456NI33fqhBnHJL2M1dPNhLRcUmX25NUb3Qmh4eKc7m5kVQc2j1ZFWzoxtEanTNNyD%2BJ4Bk1oFpzBbK4pcznT4kDKKfKxSg8%2Fisyv0tsZfsI%2BrWqjBDyUx6W4i3YODS7OwxSfL7kyy0nw14GR7St298BNDNbBcxY47FXDZhgwR3KOetJv6iagjfpr0u05%2Ba62BszmlIss2Kbxd9ZEpPAu4DpGZ17cWIehnRoeZs%2B1tykEs8YB3t0gRP3UFGTjl9BD16QDwiX%2Fzmkr%2FAvEuKFuLz%2F2l8iwcfRRnvNHLxdmfGWOkeDUEnzMuhhUUrsYf8IZzXecNJYFSy5RA8rQMn4dUW21tq9GroNihGnnjFhcU33Mcl11MmTKnjhW8ohqCaH%2Bo1rK6PJk4fR8hyTC%2F%2B%2BTHBjqkAYYueqriDFIkrZEO0eIG2UYv3z473ziEtJpR0fYHWTMdJrdUBkhtpaD3pib6YD6vde8Q%2FZonuZD9b61tztlHQI63yDNCGCDgmdGdtRwnCmTB8fuF02akSr66G9jvsP07T9se18m5Rdmy%2Bv8SpfFoBEiHYvpxploIzezZ%2BgB7JeuIBmhzuVa5T%2FfsR8%2FjUHyRdV3gb3kqsBqm6P7CyI%2BRI8f1WXuA&X-Amz-Signature=0519dd1859d1acb54c21608069dec1686ee4aac0d874c46082709be4caae5eda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

