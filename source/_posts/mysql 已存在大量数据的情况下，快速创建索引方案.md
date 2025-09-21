---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHQWZTZO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCy9YvNXzdLcrWf%2BENuC3IOUI1ilB5EvqFoNdokR%2Bw1GgIgE6OC%2F7CLMr4tNsQQ2nHGYeo%2BIaWkRS6h4JyYbJXHcHgq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDHavSz24sCh9zPVKyCrcA9kGV4Ud0qgglwzhPKUiuv4v%2FN%2FkFR442I5UCxphZoGCMGgo7BSsc53xU0JDsSUS7u1odJZ3WrzaHXx36J4D4upPdJrn7jgqO8sEIeUFhPxODJ04IklvkVCocguTy3EyOS3J9%2FhpOJ%2FmKgxTvBr%2F%2BLUD3wKj173KUw7n0D7tGdMi3XJVDOLYo1qhFxj0zREWYupOhEQE1SH1O9bf8plheGw6seArmtGqzwCLK0tgtv6N3RbEaYywh%2BxrCBgh%2FC3Y0G%2F651awJGO3HqB1OklCltK6DnhrORHWLlMk5fRt4lt41KhO6Q2dVmMGfihcaAKtI9KlkreQ%2FdlH%2FHHMDnW0nuLP7KG7UcqGY1Kg%2B5XBAUowKzchqxjhcBw56q0qWVqCrkfoUrN1j6%2FheoQi8zay1oFIIcwSgLz%2FLPIJEK%2BgX9MkPft%2B9lbUMBXgOK4Tr5nzndwd%2BmtBE00O5IyLHb5ovfBF9HIKZiKRNL59LX2u8ynKs%2BwmB6yEbKphHdBTpCjzhBZGWJUMPMOVdNA26WsGPuv024LxGXrt1s3Y9ARFma0WYelES9yqdjGA7M4YCCn6PW3c%2BJMGA1LYPZMPiuMG00AakDvVlyVE6ZEaLcVc0qvA%2F%2BjhZhNvWWqvm4tbMPTfwcYGOqUBcELv%2BuQWThIzzHeoi7kJp1lY7d1%2BpC6nZAvBgj0fCJRbWMlPzsEQj8SkmfWGsXCWTjK3b1aJcGgATEF8UK4No%2BJMdgGXw1log%2BZNuhnlYmmgMYZuJD0kwUl0vPLH5C5JBRVrB4Q8C5U%2FE0dniiKwXQciaj8eFY5SR2%2BZX3JtBkt%2BgRwwiVldVCoEQKp50BjPbNKqVn3W5YvF0dzb2ii02oYdeIDl&X-Amz-Signature=e8ce1846bed122dbf99179574ad87c9c3cb7b9f413172ad72294b7c643441d85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

