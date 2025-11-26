---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEVMUU7Z%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T030052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpgMfU0SC7zmhsVee7hgAVyj3kJHJxTciErXnGrx7yLgIgHOug1NYeWazbtzSkp7qchamAkU2Pe1wZTCfwUloogZYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDMTrhHGkBnGbRitkCyrcA%2FX%2F942yHV%2Bp5j2YAkObrAr1rbr4s6aZmdvwWQkL%2FzTMIMEjpRpp6tV1oQzeWRlyd9TmxsUDaCqsX0O71g3HqqyJ2SRX2JcBo97F%2FqG0NzcVSeCSkntfrhlzPY8EFGpDEWpMAOkK98hJIiPKQ9ZshpdBc5UVRCwoJDALsRBhZRmQst4zjsOzJ364T%2BcBNtwFrsx8nfwQvr9GTmrIM2rxgbb2OURpso%2Bq5LkiSWIb%2FP0Is%2BZyzgdmBktJPusnwPyYxceEJVSX6DbKxuvj8KDbPzyzaoO1W61aNd5d7WQiBlya666Y1G9vCN9aSX7gV96vdPEG2vhYZ6LtOsmhCBb4hMur1vDgG22V1fHb5m42zcadXa01lwG7xjzg6Igdu2Ql%2FbHCEaL8Ja0Nrb6yjqkVZ0OWmNHOikMrvpXn7agO%2B6rlkB78IknwzzKeCyNA%2Ble3D%2BvkWi9l2BzYqaWY6%2Fd9XaF9Eg8rygFDhH8ldPdC2p3Ruzh6rj7f7dQrNzSyHGsreiEbAkbxlaVK6vlzNv5ktMBWQ9%2Fgmef5%2FqVS7X%2B65v98Hs1xOYjmUaq%2B4Ntba0uqMNolpvDbseeou1WmjVWxBt2DY4FQx%2FI%2FsUq%2BQtF0BqJ6xQqyxVZlojJYI%2FkQMOjImckGOqUBuYQq%2FGdFCUgztiYU%2FD363qivioLG4PAv7kAbGj3tIB5Xaph7ODNtuucsbeNNF4N0JzVLE7hz32YHdAuGxIXcOvBqaj%2BEDFYqnHQkC%2BCVAY6tJ%2FyC72bCW1crvAozhKHZO5pnzNsF36dZ3hIExdyKuOHtqpcDXKRkUUYooIjre0Co%2FqryfiCVs18%2BFsVVT7hrq50FcR48Q7UUG0BHDqeuYXKEY%2FG3&X-Amz-Signature=41698163d303a7067ff7b6e6bd067040504e9f42da71c66d25cefb17b100c068&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

