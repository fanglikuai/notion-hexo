---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTW3GOK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFGbMXy%2B5N52mxIjIEj%2BvENK79zhVGpm2e3ulzOONvTgAiBTCk1XmcfagjB96cJbL6py20SAFueBMXThN3MVEHl6Cir%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMWYAZMMBmpP3O%2FwpyKtwDGrb367D6h4V7rpYyaN4795Or7exJ%2BwdH8v54yfCMZ8%2F7gkpahj22f4lhzlkRPoRIAlRTqAgWMZTw9eLLD5dUJoXLF%2FWJM4UMgNhLSeVtpXaTYFVU2D2Anwdq4ImID8PKPFK1I4JznxpwfffRUp8LX6JEINTp%2B6kFiI75RgN1B62HGEGMjCgKYbt6vtGeUY5dYN2lPS%2FNEjz2eFJKgOc1p47%2FTtP8FaGA4N%2B%2BftEup56der0Ih%2FFEGSlhU89Q5IHwX7D%2FCa8cog4DliJXB77ulECliQwPSGVHfu4Vq28uirPIYOkw3bKAfnmjaLlOOCdatEgyu67bUhAgA6uBt9NDVnKAAPTxfsaxk%2BHMMJhqQaChXq0WNk9%2B582%2BMBHPfDs2iuWKQ9zFGRkKL1NZdAEPk%2BGG6Keu1IW9RCPO%2FcKShlRboMC3GTNvSWOh4RxUzHUfWv5uC9fPVdDOC80CEBlNP0rsaX1rYez0idCOJxKu2Re0bBHgWoAhHNMuyCxZET5qFn8DOuPtBqs34X0Nx15eChn%2B3ibWjwz5TuBTfmJa0myHyxOskz0yG%2BjbHe0ycuPXnmpGBtTO8ckb5JDxJMtRgvEbncFk4yYH30jht7zXyTpVroVstZxWbO9B4P4wr%2FrvxwY6pgFcjzedLpzloWV4C%2F2z24YeBRoHq1GoCgRzbLybh9Eu%2F%2Fy4brU%2BgUpdaIo%2BD8s5l%2Fd2bPq7FwBFx8K44NTLR9Nu%2FBCH90HSkMOeSd8fxxcp%2BIf3J5v9p68ZpHhh1qBjxfd6gSArVZ2FwoBtGCVnbTFS6eWthNELkS1uD%2BURfr2VzBs8ZVzK2A5JazGcHkkEdY4QSYPia0lZBD94rbwRMwQVrs6XP3YQ&X-Amz-Signature=6f01115b1fe2d6a3028b80f96bbad61b837179d297f0cd9e582089065161594f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

