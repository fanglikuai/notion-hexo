---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EFWF4PY%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIGzSS58ujsxeORYUTswkEctP8ulAI18k%2BbGoeXwjZzvqAiEA87OEnDCGmrljauGRNUe9a%2F7ihBvUh%2B9ILBZSXvypHTgqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIxbel9EYyBmPCQhCyrcA1lDtPlRtcg5MDfd7zHuczUNGMifcG3Wt27TVOMg0tseNmaPKLmfAs74%2BepB%2F4EJb0R66es1hdCxm%2B6Lg4bAr3YKxs%2B9zPrlMY3v2m8fjUB2TpraTWq%2Fx5PGqlDOYSkRr94Tf00pBw4yadT%2BfAvutp4XmSGYPJ%2Bwd0OeQEZyTAtZf7Hf42e0BoNeKPSdXalrT3uJqcdH%2FM13ztGTbdxibKPwc8S6fMdTHEhq2o4FN8yJIUxLWMMlq3tdRxi7CP0y%2F0ggPphcjhqMQBotPglE31CeaiAE8YSbu7AAPqeXdhbypyVhQDojuaCMg9nyd%2BKm4lbdDrwjOTJSePMowRkT41Ke%2Fx3yaXQMgr%2FWKSf7zeHzkiPJfIWYcQHX08anEBaqMnRb5pGkqyFrEJtD9K9k1X1aU9WyeEtjYfoprnRWy1Z7uOzYdk6KUA4Ev7NodvBVWd4wAT1%2B9JgUw20snLOk8LEp8fz8tJ7OOnq3BZqwtHz2mxuB%2B7ygUcvD9crWPl0eu1rROJmPGusbkZ0N9dj5HkeS%2BR5hW8%2Bnahr%2FL2BfOKd0yAPakvRYwB5I25Kh3aIhAeFXyIVoIKCYmr%2BX0synEATNfBw3cNkISwmSpYXYbYf0B0ZATSTDoxp01Q%2BDMJ%2FOt8YGOqUB36Jy27qu0xJt5YALq%2BmJZLIhMqP%2FdhuZF362DaCeC%2Ff5X3EXj8%2B8yw54OPsg4S%2BeAuTCbsOb355N1QY0dWLX5DTZh8zMGIfTri7xGrwbws%2B%2BaF%2FNVHWxkvfjFFkSL1RRgxlHiy%2B0sbRwTEgbLbRgXOOGlppe7LwtAcsOSQ%2BCwXzGZO19Gp493PWiKQf1TeMOfLb%2FpxH1AaK8nzPdem2UZfJqtrs3&X-Amz-Signature=b20e47a5c1c2dd9812ac59f9464ce140eaa17a4d95ee1da5cd6e3786ebbad319&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

