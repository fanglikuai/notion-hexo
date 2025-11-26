---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCH3E7JM%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICM7lQX%2BMFSmCcH3rG9kZZnpc3dd8jKD3U9ZVBBpJv%2B%2BAiEA0Sh9jOo0fjGrPW%2FIkF3mFZFwFvCAeUFLrbR779dWIpsqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKWCv%2F1GG0OTszzYircA%2Byy1rBjqlH6meOkv6HOECpGbVRygBUWU%2BUpIfVn9WjQioMVRo2r0YabYfmO7ByUgKKEj7nH5q01u8bAIZzbu5wG1hJCzVGBzPPQI6bvnv4gfRAp2RjlQWZ55t%2FkrDsLB4Fw%2Bp5CMrUx98zOZ5MvYcnvpxCCT9k%2BUvisTVsH%2F5k0o7PNe0JicRRG%2FKOVWV%2F%2FRAAY4ldy6m88OqPN2HI85GtBBpziM1ahdYa5cJlpfC4om2ZmHNwHkWYFupk0W3ziQRye16GhAOkGUP43xuiUWuctyJt4phckYNFa3zCo7oFDt2JMQ88cK%2Bh1GJcO62zEgDYigDwOhicGxDDmcbdPSV7DB3YJ0K%2FmRNXWYbJ1lQ1qqTr4UX%2BdsJ%2F8mYgMKx5gjq25u4ABJZYij74RfMYy14a1q5rbSvHjKlcWEaUvRd5cJRL%2F2iwLEbayG2ZxMx5%2Fi1BeLyMpG8wminlHvblSLt19WV%2FDqTNBAr%2Bthr%2FN4i94rqpToL3hbaKi3EP1lYP0gx9O2g5n%2Bil65mIEAK0HCx9GxBKNQ1zNHy%2F0uoSJHsPNsO7oty4m2WyHUIKtGF%2ByJuol7BethW9ZoRai5oOzYSDihpxPnuc7WjWXF5X2NWp9vf58%2B5cKIwbxtuo8MJXKm8kGOqUBQP82l4M8URZ83ze3Hk5%2BBUDRINGv1muymzq6tDZ%2F6fCehtdWWxscAwuppTyGUBU4WWjdiz7mOrIkElYXeBqQmHKjNTc2fCW7nCkUeNZEUxLhB3c324EfpVwXZYiV%2Bm8IO6JLBycJUl%2Bu8rFtUV0J4aBTwMTS4yBnDsUvOIL0ZIGIgl4UFA3YNqujKm%2BjDcbAAO5ANppR3nwmFZFKYR3DAjcPYI81&X-Amz-Signature=996c19d194c44d82c5022534ef92f8f1733a379b5de9fd10575c8fee4f993461&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

