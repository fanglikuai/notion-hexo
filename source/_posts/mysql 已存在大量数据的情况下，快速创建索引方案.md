---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YEXIWOS%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIBneR9UHwqWsqdcLU16Y2nb3z5Ka%2Fq9riQNbK9q3l6nuAiEA%2BoNJS9WNOKDaF2jWQLeCxAULRjRbsZYHmTyBBy1uTmgqiAQIxv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6prd%2B4IgtreJKXsCrcAxjo6De%2B4mf1hX%2FsllSMvr9vpBQ0x1KyQzj7EnEAd4pSrI9AOxDhKEUeJAAOCauzli2zkIAheKu2mmSUIAijWEKFcCVb80GmSiyPsRqSKbn0UCKwZR%2FaXlAqKgyccke3z1j%2B8cfuI7YSrSy3Wif63XB0ZEEUo5RTh2UF45Ki9uVqhqVT%2FcbW5OOIklDIY0v%2BYAaGRFBuCOTNHHnqiIk4LCYLCJrhwMqMlKeGUE8Gkce6ehT10zMjCVG%2Fe1aOgLglTTkp2%2FRMdeWgNdu7z2BdzYsO6zSl8zF7AfvcgZRtN9%2Be8NKzuSHaK%2FWglLsi5zFA1PPo%2FwUGfz0B%2F7tL0N4%2BLR8HH%2ByVq0L7q7PuB85qt%2FWQRBLpBEZIGVcZqaQ5YAp%2FDwqhK%2BUwrVR5mR2AI8jC%2BUjpimFjUYMnt4Tb5OvyLYCwR489W%2Bc1GCxgeecK%2FO%2BOrrx%2FkCnkRwkb6erKFO%2FpaRGWDbqhF7AdXCZ%2FYlhGbDnDEUv00fPWA9lWmgmSH6TlhwM8MUmFF2v8PBsHwmKH5bmP7eWxohv%2B95nReFlUANXIXtaVK6vsQOI5wBO0%2FMWl3ajioMEovFeUYNxtFetoQ%2FLKt2cZCy6egeodjqFJveUrgPXltIqMprKnsdwfMNCim8cGOqUBst7IYRrF9euhR5yBbQC3WjF2o3Lxv3KSgyPJg%2F3i1vOCqsJbJNNcdbwh0MfExBJVGKKWuVrgq27H%2FfpUPrlsg1leyc0PoGkdEIAM5e8edU7KvPNAZGkVheC5MqP71j2r0QfsAgFCKn0w1vPzooLCySMmohKI70MJBRgkNWZNPk68qy8sSqPkZuHcPBMrcoAzNMhdBFGx4maUwJxhSlTaFe98A9IK&X-Amz-Signature=a1f42cd2d2803999b1f2cf00aefe3d3d907625152e9ac1d96e4cabc2e157ac39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

