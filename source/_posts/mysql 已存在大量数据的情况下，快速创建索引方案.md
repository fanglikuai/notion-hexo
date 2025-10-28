---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYXFGVZR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD6x2vX0sMFFpt5o66BVB%2BYC%2BoNrJ%2Fj7bRza6LpK1qD2wIhAPcSNoH4ETGhWygXA2lNLcn8p2mjk1v8wsImYidAvWUiKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyjIiNwdrStUvDXdtsq3AOv2Mf%2Fygd%2FASWAwQPdyqnr23UigoaFJc2s3IcyU2eLPF7fhgjXvAL8Nwbz7ncCI7vusGBfpEWzLX2sGh3u7FO0XJeyTqbxcQsknnsTth74D5v%2FdQxlKtwi4lQN7vMvcswZkUoSaV07SSBZNlazvXj5eMUWJ2e4L%2FZ3OPnfUeES6twZqVvzCql5xPrpOxxAzKaZuYkbK72H3r3X6gHftLH6q%2BaNmP00s8%2FEDV7Q2mxjdLkI2WbUJXQjVgHYQpWrjtxw5XqMb%2BNGWHfQg7LVFslHyqhNbeCYYUYdyGWneQ1jQXnbWYgbaIGfT9%2FENU6P93X4nDQhRUGrJ3v7npr4FnfcZMwwo9B2n%2BD9F%2F8wBSySa7x2Y2PyX%2B%2BXwBamY5wGyDooHibnGY8kFYl61PgsqNLPbqGOXH5Spm%2BrwmlQVr4JWf7lgGBukWSZBnt%2Fd9XRjMzICwVwSnT7JouoPbXhba9Wz0dgtTZoB7wweXhzPGETxWmkSC7uyAaX0aiu5lyrb%2FUVSbJxJaymrEbk5%2BKrFp6R%2F%2FUUUN6onVLSPNiyXJNtgxXdqU8%2FSMO9b7iwSNXRI1QlAb6TlLytzIUKrFrzU%2FaIIOKzcE%2F%2Bu8EgO35owSywLSDJBwioz%2B4r2rqwFjChuYTIBjqkAR06bd%2B%2F3N9PVbe0vNk9QzrZTkIB8e8oLpKfvN7xsrb6wAFRHjnv5UdTcyF%2BkHd3NysE5NS1jlhwqCUvC9AGvS4xLnmDAY2J4qJLOYNJjR2%2FICgS70lXmrjPBrTeJ%2BsNnK6ggsg%2BISEfVF8M1wVTowBlpbjnW2PguK9CF0W%2FP5mfWYlXmyAMCTF%2BYpQX8XIgLSeYKq45wxCKvTIEXRBE6HWMIY4u&X-Amz-Signature=e0b52835f4daf5a74dac021c62ca0a9ebec4ee725716b7771a5ce1e7a5863776&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

