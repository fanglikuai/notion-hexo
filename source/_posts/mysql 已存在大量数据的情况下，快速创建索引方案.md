---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X2IJH7U%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIArbjS1RJv7O8bSL7WqYWgoi7PxapY7Hlkv9SAg35PEdAiEAgm8fI%2FOuwC6RcIRw9jAhs3VXu4Q5BDhQGXfWEsWsnbkq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDA46e%2FMcqW9GZ%2FCaLircA5PoQVn8JyXIHZev9bjW8D6K63lULLorOrBubxx90R4HHyuEwzLtXgmHNNPXROsRYovcJLe87OO67aT0xes%2BmZHOzt1KDPiyLKe0UJK3PtfDoqCFsh%2BdcfOwhU%2B00eYqOf1p%2BSVmwOuZ8f5AumGK65oJNnuIDhOtSIm1boxKLJ%2BI1JBa6REemZu2KqJElIc4JA2CGtc%2FHVwHmrAx7xUy8r8ih5UQhT%2FfNb0WWYuZqsclkwYIzXmYxSVlGxCaur9NSmMK%2Bpgw1%2FYh%2FuVaMmzJKnKPtF8jxwNIwAXXGgSD%2F2JF%2BM6MyhZVYE0QCGcNs8V8EsEGHHG0QqIsIEGAdto1sB%2BkMVxRLAoTJ02CIMCc8vjmy%2B0OOIooSSAkbOiYKBe1nqFQgre4d9cSq9tzLDnJ2o2EXmmnlabYFr2zlZNVTlNTydrTgTid0kVNyIZoD2hpGOrGvmeZE8tObql5iWGeJNWZTPf2ZrTRtY66WBJGIq4hukBbL46KLQA%2Fpbj4jfAPlPYJ%2FHUBiE3vG2kpEetvvoqjjeqvByg9C8a5etnT%2BSizfphG2ob6vE9PASHSHl6lLga8h812675iygydMo%2Bu1VmCReDmb98IeHSTElpBLkXp28C%2Fn%2BAYnqqbEpCKMODQ2MgGOqUBYNk3d3NQxkOYEPAmDMRR5wSNCy21ahLlnMMR3vXl%2F5CNGr0XtS2SoPF6J04P0pXUtZ3wjICOdBvgetxh2o%2BtaO5Rg3jFacsW461WC%2BLTojV6L0EuPzlf0m4O4TX8gLX1M5lqGoMuTbEsaZYSvY%2BqJ%2FHxNW%2FJb%2B8ofmbWB28eQ6omKUF6R%2Byo98AKpIooBq%2BAWuO6%2Bi%2BZxs6mxfd%2B4nQsJGhF3QoT&X-Amz-Signature=257d9399c0c6afb6b65007ef190f146169316e44149978209d6df8806c7aa522&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

