---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663C3S2JXH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAsh9bW8udsh2jRVJ4wnGv0JX9ZGrvNkT7ybzHweY9%2FXAiA9KmLYhw40NdbJkC3eMqV%2Bgxn8hFYzXnA0P9JPdmHJEyqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM38Rx0CMerhJNxOw6KtwDn%2FLpD61c3e8uASyDb9N%2BhFoQsjcPv02UkV%2Bss5kWmuaYcLI%2BCkNu3yZuWg%2FgQ8zIN%2Ff%2BosO4BbyHQhKRdMkKhQVHAurojVhMsjF9Rxy52%2Bgn%2Fkimac5Qx%2FJeGVnG3A9Gh0jpFzz64wxvgknUSHiu%2FwX2KuzkJZlD6WPHGHc9EnyNraBhMP3yKA2L9z3wI%2Btb2%2BvtJFSsIRhfD6%2FHEejkmyYvST%2BLK4zP%2BT9CcogcevOo7g7H7rKnnHXv%2FNN7z66euuw7MZZBCP8RJMHzU%2ByGKrADsdcpEjuTllCfdmLpje1f7cp2yZEn74mmuoxJFC2MaFd93igUYkscEP6xolf0%2BoBX4bGpLttG5HcsA96Lbmg3vx3ucnwCAz4%2BzrQJgyBtSJ8rdInzFLnUFaZvjKZancIkEaZv%2FcNstG0e6shFPNO1yUPa9N%2FxDXClBPa1alQf4wxuZx1CGH0CUV9FjQS%2BMRLUUYAgCWwibM0d%2BjkAh3%2B%2BO2qhOTw6fFk4i7PBkJcy0kQHQ9z4t1mz2IhVMZvTYWe1c8mm%2FrvpfrcCGAo3yMmvIJpg3yT%2FJYH1rYZwKXgJ1dBE4uvkC1Oppfv6l65rQbzlLxkVVK4g2oRGJGwq5phOApuB%2BvH7WWfdc%2F8w1%2F69xgY6pgGM6yGfuSrzolfAzPVbgWqW4rEkLarqlVfxi4%2BafGBaFLS1INIfEnfJofez5ytK9Sbem9nL4omUxc5Q5lT%2F20VO0JD61%2B6zMOr7ph2DNefdyJA9AsVKlJNCp1YSeyX0nyxSeLkb3ueLRvRl65QQVGp9%2FOBYrAr71CBv4txMQf%2BxNMxFlFRcfSbXhzXjnobOQpJCiR8go1Wnh3T7BaQ7pOe9PN1buqGc&X-Amz-Signature=ac7fa175b016a8520cf55415f4813ddc5ca414c31162f6cafb4eaa6301ff0cf4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

