---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WOVZTL4%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG9qDtU%2B51wYnHSjkceno5dji8IGU4dFax2VFPwbu1hCAiA0Q2TJQhjMZJm0bZjn00YmKM1ac8H2mYwgD6urRtDj8iqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMezduoEizk7MgIjYKtwD2ArEVwMpX8KY%2FK6nKeETy2NbYsNcfvBW3gQF%2FeJ7kghjtgoHPbEQPhN4cmf%2F413edwQpcwmeXTo5ILeZGR%2BhUkHEXFpWBGaK1wxwm3a8CmicrWx%2BfUT75CHCo3dYQiP1GhRxkQ0h5QHqqn%2Fp6v76Nw1bEWnCYD1FTcS8KiHb1JiIYb71JEKsWyRE%2Bxllu92I7GPoVKyDGZ0Ftht1mN12Ng97bFHTKrzFJr%2FtnPKbUiKfx6MMO9fbioNT9ZsosepycG92%2BMBsmTpw70fSreie1NFHiN9h5PjqJ4YNKLfnpP5DWuu04USkdNEYvBfPb1PDKDAbPMpRM9uj2f3oi1lS0IGIvgv7eRHX4uYLUe7iY5bMCB%2FAIopZVecma%2FCnoWJpbhdwII0xYNTekc7Gkb3aNIICKF9mxdbse261qIb4japHiTDU%2BBE7yD%2FdgCzQbl2vEbmKYU9ShSDQ4kQhuRLzC%2F2kGCvNw2scFsx%2FGlmzTWvelynWyIw6Hv0EOdBKpfJ3B%2BjPp6eujm53N190gq8Mhh04NZxtRqs4o%2B5Ru9fKT3tBOA64XTcrA%2BatDJ8iWSPxjy0bqY%2FHX7PiXZrSNWz%2F7MvhNtlv7ebg1ZGMqs0nlqdZHyeWPnKJiuL3m4UwoaSyyAY6pgGztogYlANf3sAWXviBmo5OyKnOe8bv5v4nTnjYkYAaPQQqfGgbKNAt%2BDiwOOwIUznpHIIweUiYsQfMqHFyBjd7QdLGCPvOvoJ2G1qJIVeERKFOd0D0p%2FKCDC3kFsMk%2Ff7cwoRnM4wlrlibQAJB%2F%2F4RQZw4gV3cCmTPAAC5vTJvjT7z9GxkUEYcE2tJUjiJZ07EFhE8eY0Q8BHeJVgRcxY7%2Fe2IqLHB&X-Amz-Signature=7dee3e472d7be91c89f51304a8a6231fe41ff38046c9db04429698fbcbc28510&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

