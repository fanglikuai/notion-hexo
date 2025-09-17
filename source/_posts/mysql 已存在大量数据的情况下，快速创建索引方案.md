---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVYQZ57V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIQCEEctbmWQw2DmiRZKGHjR%2BxUAvZZCosR60oHWFrTylSgIgDlp24wWlGDy8X0kaVvZIqPBdfU9GbN1DFKRpiZS3TWYqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAQHdpU5EpWooQJaircA%2BeKC9lK36CJ2NnDqf%2BquppdmqFnbshpLp%2BZKCDXRYXHGe7tOY4CuJ6iG82GxnqIFdjHGJ%2B6h9sz62IKGKewIrK2Fp37ApM7NRu2iS1XIGC0cN%2BXvOI6vg6HMKLBr3G7sZloUn7yBQ2xtzkdIdsJaemEXA%2BE51u62k13XpHZ7ODFNE%2Bsc0YlkhW4CqkyLqCUpJYOwBOzWEvMmXYqnwfJxD4WyiGV979La6lHfcJY3UAMN0%2FfIfxgWPZgHlJqKvk7U0yf3YWEZkOn9KQM9ZQqZVc%2BzovVdsKus6LZXB3k1bL73n%2BQ2RE%2BPvUFuLgyKA79Yu0vB3CZqie0ZAs48FCSA%2BEPLDLfmzdak7WmY%2FVve8%2FqCOMo9%2BqGGjy1AqSoGU2RCGldweZ6o3O9bcoxkKzdgGMon8gnic9zMBK356atGlLYYmCK%2B9HQdJduJF3%2FA%2Bw32t%2BrfqExU46uQje8uKFGq6LjrAa5YwBheFVP8wJG2t5cY74PgCUIuZjnAgQdCejjN4ndRTIUOfbziYrc1eaUnYysRn54ZuKuYvFaBLXJL%2FOdGtFnItO3Tl2caf146CNq%2BtfKrRb5Mko65DhuBDV5JGLFX41P38KhE081OVEOt%2Bt%2FfN5m5H6TjK45Vmd7MIyyqsYGOqUBw26wc2PYEs%2Fjlnftazoz3jugC1j2JpQQphEIyBLbR6dPh4RZ%2BHWCm7S%2Fd5f%2BlnIaUOhtJvWwQ%2FKN1XF8H54jROU5Ay%2BNEuAkwJWdG5P3Tbf5EUY53v8fLCGBYbZSp56UzWluN3sPROOHYjys8a4c4lT%2F8UCpTDV0JNjvf8m8x1mpBLJIh6%2FPPz3goAnqkc4J%2FYnT%2FHa1Y2qwRuVmSaVBc8QYBHBN&X-Amz-Signature=1f4852d4e22ca416b9e765b0e8055e444a455ffb4477dbbb23d9b0d4852694a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

