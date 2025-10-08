---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q243JJVY%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFCahMuBUkOaCokFYU424ZMcy3yos2Rj9ups%2FPgCqdjYAiEApi5aUqT5GBYC7Y1Rmeqp1QBGzylxVhxVTKUUamhGGYQqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6GkA9aT1OGTY47FyrcA23BsUd3z6lcXdbyv0neHi8eA1WiV9XUJyL6ML%2BRCY66bgJt%2BxTwEl%2BmAKPMKbRwouJDQtgUEPdM%2Fl06BBZU5M4tIVTyaFxIryz48NcMiq7EyIWm0S1bIbjbap6PtxoZkGxpyU2dcFuwxA9S70Z%2Fd%2B6W7cTD%2F4qBSTYUuWre5i60oUt9NSDPp1S3%2BiUQ1HjWPXdwnPoW5Omm4nyh4eRF3K%2BT7zXr3tq80lmJK%2FdimMhWZSo7HePgVXRzDrfVsftrczQajXbXtgBuBqwJNt7eY5MKjh%2BoJMmu2PX%2B%2FT%2BejFRi7OTlotkuh9d%2FwQa4j%2FNTIJIeKyDTrvy3mMm7ILa3baxeMkG3kVN3uM4d6zbbIDxsy6cOF78pk6R2V0%2FnT8gbv%2FAukHx9SS8lgt2VmQPCKT7yHdBFkbeZHCtHtMpWrjpgBSoIK0%2BOdESfYugIewFAui7fhp3dC5E10T2d1e5ovK6phIWUZz6I4xMutXEp6TWd0L%2BhxBT6EJH%2Bz02tgn6onOh9HOgXnYMdUfB%2BGQxT1ur6whd6DiP6w0DdX9b4%2BFSu5wL3JYfJaBCS6i52Ts9pZUWri7rdG5ohdNZWk6kwpriFlYVaZ0%2BNxAwHXdxCRlzeDSM%2BBQIyI6SePPBvMMnQlscGOqUBX5puYZ%2BsAx5JUnctTdKYfGKwukBdP8lAVAoIwqLXKjg4hdE44TH%2FmVaOfOa78X3ByS6%2FlUlARW9oLBqnpCSutCglJ5EsiJKCAozP8P45uaXsUP9cx7OQYxpVqM68IFx3%2Bi3jdIeCwRFVQE1yO3BRneZJipOu6XEft5S8ph3KbCFTfmvjprhpL8GCpap35um9Fx7Qv138kfZZYLTo%2BSCFqqi%2BObEY&X-Amz-Signature=a2ba63f84e85477830421cdc0138f9fff13377eb37c0a63ba5b2867777317126&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

