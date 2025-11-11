---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6CDOKGW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIBdZ516OGwQlrKOQfOztAXzvWvWy6SxGeXcELGIuGNqKAiEAnQbDsGPC0SGVUqnyqnZPQuFAT1ITTIU7%2FsVGCcIFy5cq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDC3CgI7IfnOWDH%2BrMircA%2B2wtjV9AaLxiSZZI9IvGxv4J8M%2FBoLYSFTqRDNjkSyp8vyCOFmm19csCF1jzZtNiIT72lPLo7QP%2BnH2tC%2FqxZPoxRsCSObkMTpc2UT2CsUSKPUO77MKNSgmmSZM%2BTMnVwYGTG8FFyHpQU1w1BdmRmEBV%2BNfWC36RWQnXme7g9hr7GQvbyuVY1P7VDPb7oVE76FluA%2F757biOeOJbAusFjcBdmuQGBMUrehbayyIIS6RaoBeN%2BhQYv7zY8eFgobmJ5NZZZ%2Fjd0aSg4tSQaPk6Ac8d16cJjZuSo0dJvHuByVZObUSOUmvJwkoLbfa7SfdNymr%2FYm%2BbdBrO2h5qx46p6Y2Q%2FQOL31zjcAuX2m4Nh2BJF9N9LGUQRa9ue%2B6RMVsyDTkrkVCqGfmVCmbkZfFSoZFIcXoZuoW5kZo%2BnltUY6vrlpIkuz2xDpBhyBSHd73NVVKfwtSeP9eL9Ddqu0l2MwY6qK9xjfceHRpHsPfIOOkOBfmJZsn7PqFXaZwHRMqTd%2FGBpvcXgPiSKp0E%2FHZCPtOgQw4bwkE3qgyIjYczRpOstc1iXQY6SZYbqmXvGsC1ME%2B0xiXqqm4cqewiViS4f2NjbtEerc%2Fd%2F5RwmWEzkEKCtsWRlsAEkMSbMNzMMiozsgGOqUBUPpZAucFW9ic8eoVubdVUzN3q5HXzIb6vrj6nr1cyX7HdRG%2Fa6ezP%2BjcvOalqZOiQVxaAwAFS0mOaxBSXGsG%2FjKtV5EETHLyYI%2F3rntA6Gl%2BgcraUtmIvl6bT%2BStEOrfniavW6O%2FjzwjHmFcizSIKCX3EhsWloNk1QlvxzZjRZFH21ShUzetW%2F3AJr4Wdj02JkvOC0ehHmDbcsDaKljebZ8IGCpH&X-Amz-Signature=47535659096ce0d3a2201e7681e4d2e240769a835f63707dc27b7abc16f929a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

