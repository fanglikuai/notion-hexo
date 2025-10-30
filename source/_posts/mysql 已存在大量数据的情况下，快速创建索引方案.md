---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4SUIJ62%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIFWmXh7DVIxRnqIFq70bBFKsuHGlbO2DvVR8B84zBq3fAiEA9YC4hBkaAg3AmjvMb8aoaT0agmWDeAX7rTjdHGyKcogqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI1LCHCGVY2Kk6Ts1yrcA9WaPn0F75KqKalkn8SoEWQRgveWH1GN%2Fg3CZNeyArNinzxqiHXaXY416zc8g7yVpXXYAOT9MoC2bDTk5oAz%2FKmdY8cuNAOtG0mLN858uGs5GgIzY94YAg1dSsyefaTdI58ksov8MI1P1FEqNFeb9PMdvrJde5Zn8rZWDlpE6Q%2FAz%2BvK27NGEJwbUSYZd8ZvIxLJuuOaDHnG%2Bidx3ZMk5bS5UpkEKKYH1jcZ%2BlBf1XaCT1OaiCTvMj26wrMk51%2FvwPgcwEzmPCqQ0LR6tmJ%2FojFNxuDEtOePLLhQm7QZn5pF09K2FYbyUP4WlOiWecnGMGb6%2FGDsmLWNl9SgnY%2BdPYhnYUtUTGf8C%2BXrHpG77On969UIB%2ByB%2FVRjRMQzSU9ysLv15DMbOhaeKZsqsZ6aSS39MFcmJZYPYZJV%2Ffqhm56%2FakE%2Bn%2FPmN8eDlQ%2BsjXvE3CFbtvF0eaTPgYf2aBhg%2FHtYcURXcL4krunqSC9PP7FostrNTh5gn0Px%2FAN8Fi8I0M%2Fdl2pjA%2FqaDEu7n8SNsGsbcaDHyfPvck5UlAhbeMBS2xqjAAiyfcUOHKKIRZbYzakBR59xBEJefud9kC9gzm4%2FY%2FBFOUS882%2FB6IQ82mBByovTXvMQUOSmFlZ6MIy8isgGOqUBvFxJSIoYJWYas7Ewz653HCycOX%2F8uy0IjxFddrb0YLCY7lLYnX4vuNQd9pNbyuTUztME8Ur0bXUSniV9HxUpw%2FLISStuL7DNQ1cBsA8UUGnRzieBwWt1JuFnr35BqCueclVeZTm5tAHHtWVC8i1Gsd04J1W0AG%2FBWWkqbxxl0Wvb0VP0nCZescyiVJ97e5jUakj0W%2B6KftidwcWOkt2BgaP8kjaV&X-Amz-Signature=edacb852ae2beb1d3cecdfc1912e6de9752b981f5ac3f1e8c6bade020e138960&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

