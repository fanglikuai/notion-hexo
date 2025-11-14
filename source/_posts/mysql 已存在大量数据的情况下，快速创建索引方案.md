---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMJ4IY7T%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAPr2HKg176zfwHkf0WLA6DhnfiJAsbPfgIflLWjG39%2BAiEAhIRKwW9pxW9ndKZlYDJY46f%2F9Q1vmE16VoTLlGjbnV0q%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDABOaxYkaSfJSsph4SrcAxrvqBVsqS68hhrjH8pLsJWWqa6MIr95x%2F3p8Fe74dV2BSz%2FZ5fAfprY1FlMzp0canXx%2Bo%2B4sjzWjD4Q3EE68YGbwGLMRCw95Vb2urZQylImSkSewHbprg0muMk4yztWZdlv84uCaLw0dddXLuYLNgxQ7LUVPMFnbYejecSl6D%2BG8T75HYuxWEeZ83v2t9UKJRiDFNxSNFsH38MFRXbCTq7cUegMVqbvpWGXFh7MPTdwdWRoMNwjXwbeXN57gNvjx%2F%2B%2BfLM%2BJB6mMaTWWMJ7g3x%2BUstNP%2FWPEjWxVzsIb7qdDTANnsCBO3fX3dFw6ykxuzNI10pBbQJZo%2Fp%2BvDk%2FXYgY37X0bUBS24Js2RuW%2ByyE2yYoThC1mHVFEuCyI4wjnMUOxBk7JaUXnDNbSjUG%2FKr64buSV8ZG75t7cQVi%2BBACs%2BswbWFDrpX2BRKZmzryavN6L4llMIdHrQsizRmDWC50slfiBgXPs147Iv%2B1EqwDk1ketjAd2EmnoVExm73%2F%2Fb9DvN6YieAhZuj3W5KfVfSHwcKQit3gBdkpHL3%2BFWS6yVwpan37KxoflbiFvbfBRlqUQyvtXvPuaKpZQ7%2FnwS4OOnhLzBGCVERO1qztxtpkiPM3MYC3K8PfDvvIMNyi28gGOqUBoQct4JeHXIg1xZ70KxkK1O7lMlg%2BJKN%2BQ%2FWOrQ1QITavAwYBZ1zgo8%2BVZbL8yoqnmWBCCJ4bSTEg9fc%2BdDYuW%2BSmjdWMYAG7R5J7OdCMATT%2BJym0EmWKbsQVa2LbMf69r7kucYAYNz4tIjamsSqFkU6W%2F4y09V4n7t%2FArLHUPaRJf%2FLVM83kssYG7Ok%2FGFGhEF8DgotsoGpSsM3TJ8kySuK8cI3L&X-Amz-Signature=d5a2fe5854d0f8c0a8b930f9050424dd8ea8ea687cc168576c93219a67ddb066&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

