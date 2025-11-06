---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WOVZTL4%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG9qDtU%2B51wYnHSjkceno5dji8IGU4dFax2VFPwbu1hCAiA0Q2TJQhjMZJm0bZjn00YmKM1ac8H2mYwgD6urRtDj8iqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMezduoEizk7MgIjYKtwD2ArEVwMpX8KY%2FK6nKeETy2NbYsNcfvBW3gQF%2FeJ7kghjtgoHPbEQPhN4cmf%2F413edwQpcwmeXTo5ILeZGR%2BhUkHEXFpWBGaK1wxwm3a8CmicrWx%2BfUT75CHCo3dYQiP1GhRxkQ0h5QHqqn%2Fp6v76Nw1bEWnCYD1FTcS8KiHb1JiIYb71JEKsWyRE%2Bxllu92I7GPoVKyDGZ0Ftht1mN12Ng97bFHTKrzFJr%2FtnPKbUiKfx6MMO9fbioNT9ZsosepycG92%2BMBsmTpw70fSreie1NFHiN9h5PjqJ4YNKLfnpP5DWuu04USkdNEYvBfPb1PDKDAbPMpRM9uj2f3oi1lS0IGIvgv7eRHX4uYLUe7iY5bMCB%2FAIopZVecma%2FCnoWJpbhdwII0xYNTekc7Gkb3aNIICKF9mxdbse261qIb4japHiTDU%2BBE7yD%2FdgCzQbl2vEbmKYU9ShSDQ4kQhuRLzC%2F2kGCvNw2scFsx%2FGlmzTWvelynWyIw6Hv0EOdBKpfJ3B%2BjPp6eujm53N190gq8Mhh04NZxtRqs4o%2B5Ru9fKT3tBOA64XTcrA%2BatDJ8iWSPxjy0bqY%2FHX7PiXZrSNWz%2F7MvhNtlv7ebg1ZGMqs0nlqdZHyeWPnKJiuL3m4UwoaSyyAY6pgGztogYlANf3sAWXviBmo5OyKnOe8bv5v4nTnjYkYAaPQQqfGgbKNAt%2BDiwOOwIUznpHIIweUiYsQfMqHFyBjd7QdLGCPvOvoJ2G1qJIVeERKFOd0D0p%2FKCDC3kFsMk%2Ff7cwoRnM4wlrlibQAJB%2F%2F4RQZw4gV3cCmTPAAC5vTJvjT7z9GxkUEYcE2tJUjiJZ07EFhE8eY0Q8BHeJVgRcxY7%2Fe2IqLHB&X-Amz-Signature=14cf7a08ac7dc869a1a0eb509804d24f811e01ae804f78762c543728d9960733&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

