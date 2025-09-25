---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHTLKU6Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHlcPGfNU8U59Ay1EEeZoSXKtbxH0w73dYMtgRoYCeavAiEAq%2BPvQgZRlNcGymfByb4sYIRNaUA%2FLzExdE3dPcJdFXgq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDBUd7AHrimbWabKEBCrcA33apEwHYO%2BPqcbkN5FbPoBvcVKktD5ZTKbk43AzJ%2BxtCVVlCNC4idJRzd3LxZQHXSv0CrLX%2FqvKHhSVjOjTRZcWj9bnnN63G87q8CGzQkznzaZrnJP4piGgA8SUp9VkxSij243YP5L7zN1z94k3fC90JQDZFe8C%2B%2FeCAyjrHUDETaksuXUrAghLIBwPDjd2ST%2F6%2BQzOjaEdOlE8mFs6IQrL5JrP9%2Fw8TLkQ3A%2Bd3x%2F%2FNHDfSuP1ue7T06sJ49hnFAD3UhUu0%2BKcbdzUXz%2Fc6ydL8tPKHK585Y0KYlgCKR%2BIqQ8PRw%2BkE%2B6v53okEISu5RvusjDpLswYhKdckW35Gg2NO7e0E8dwOkazngBuT2uLxitVGll3uhEJvzNcnFCZo7Tok5ruynJicDWz2JrTOj3ht%2B%2Bpb%2FOavoCCnLuUsuQF6lT5Scg3esyMquFzU%2FUSXdsk76O6S6IeQSEdQNf6IZbPzgyBWfezbgNzR690XAFDEVgUVhqMAwTtBg71xLV5fcSuL410qhhw52AONtJ0lFBdOIGdepRamJfb2lXmmsqLmCNNp0fxw2UY6L0aoPAV5b%2BXoeCR5XGXJJ51L949jmjQum8bJ3gLKv5SyAXDWx5NN%2BqFeZF4WLXSPz2DMJOI1sYGOqUBgd67G2fd%2FwE7SBHmN%2F%2FhP5GrIPeqFUGcK1IxU6vO7Icc4SgkUH3O1BtbLMbYqVHv%2BxF8dRYWyfMvpnGxTRMTz19WW9UFNlCb%2BIvt%2B0K2uHuthXUgUZutNGi6TspL%2FZkyuPY6O76Ok%2FY3sELmMfwiUDAG4IemRBE6CFKs9bXmYua9YiZikd8y5dG8KS4Ade2Z8Ic6qMj7cOtsL7%2B0dGGGF9rPY3mZ&X-Amz-Signature=1a73f5758d35f854513adebc208557c58f9b2375bcfc2d5cbf5330d692e31e63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

