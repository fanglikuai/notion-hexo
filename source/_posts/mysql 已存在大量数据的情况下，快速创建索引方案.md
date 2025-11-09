---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHK4AU6%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIE5gHKmLl9j2bdIcfFSiRfukUqkpK7wo8jotqO9SyvjUAiBKzNs%2FJxOlfVbyEDvJ7CX%2FKger3a60mlvf%2B4130qPPpSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrkJOu%2B%2FQWkIL10eOKtwDzdP9v092ErMDXPNTPlj3qXlThiooP6vN3811MLREbBPWD9Fz357Y4%2FjDX4h%2Bde2DIFNpqcqkaiVLHdXbD4Fu0X%2BnZ3NtOsFrFfOoU6sBd3r5%2FxZgBeatdPjufUiKCKYJCTgi69SK2NUrRcKopzPnyhNB8bCiLhqkGqHxDgHKpEnEb1A7topS0FeaNQ%2Filk9hSBjunR2I4wBvtqFifGlpMmEiz12EFxu5U99JqqddRU7niiFw08C0OdBF3SL3LeutWxbxmb653bfpeWTnurV1l70s50r%2BENB8Rj4zFu8flOIa0UobN2dRli3Eb%2Ftky7f0q19Sx6P39FAuza580WZqGwl%2FKcS%2F%2BxQrg%2BTkpjzgkpjnaGuQpO%2Bha6gQIXeL2XLjl26GWosU7HFny5Z8d4B1Hh0grYkvi22bLde4Uauc9OYcuBV883Qi1mdBlPxdwg6KE085mIUIwImychxStOcq8lHSc4GceNWYfK9UiDXRrxfShC%2BgGwu%2FjNGzaLkVn1PF8LSyal4ub52U6PsLnKL964uNpL4raUWhQnFNhSJOFutmlN2tWrHJ6rqu6Vpwdlil78Ipw31w9qWinx5fzKCLgpw6%2F7NMF4apQMLFowqUajSGP7bwm7O4k0DTWjMwmozByAY6pgHgbkeutQmsR%2BmUEIZfS%2FkB6rFWEnH37Bhdj8J%2BL8TWHfo8KozfacnzuHcuCw2zFOtRZlx4N466dxIQO2IT9DXFO6Aw4rZmo%2Fircw1ivYiOVD7pU9FvKMxNJxBclBrr%2BwJ%2F1Av7NdWO6WGmYcrO1yLmFLK5SckoyXTBg9V9Qu33dNfzonDmYngBj6MJN27SUeZfREiMl1eXBSDvTxrOXBWZNGCC1i7%2F&X-Amz-Signature=17778a4b77989091002cb0febfc88c049abf899672c5a8a1fe83b1e86f40a31e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

