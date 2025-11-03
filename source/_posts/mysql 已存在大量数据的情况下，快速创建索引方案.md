---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBXJ64YK%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T160058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2V0GURuJV7XZ6jTADIN9QrYSvqJrD5RZvG2Et%2FYJmbAiEAzbJtO9kJYYrSvy9%2F4mkQoE4MBOUzfgbqkpvi2EJMAIsq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRAfK3zLEX2GUcYvircA9r%2FP3ht4g%2F1P1x619NLZprYucy1a9b0Uz9eYYGZgSeaW1aBdh1BtJKBGfOYyOJVnJ0GettK9cOFmP%2BDN%2Bes70wg3mAdDlKEJhaQbKYVBR%2Be4Qe6G6l%2FZPnhhmH%2BJ0scdm%2BIoPz5C1G2uPbp46LztrS4j0wPaZh2sI%2FU%2BOffNX57VMN25ULvvedMKafqnicBW4XT4H2v2PHiBjN79HK0cu7%2Bb3TYtE3ogtxnAA0YqNhyeE7c1%2F8yKDDS9zXZk%2BXrWcInhMVc1TIYOciGAG3pqN7WqG2ztpuekVm7%2BtfIRjaztZYycRyCQqziKYTGefq3jlgyNGlWGdRSbaeoJSLdAgydLE%2B5QtGaY7UBSoiL9mrl519%2FOCYwrdMREXq5y7fCwkH0o5q8e6wSFIKaPJQmrQ7SHgwTlFENVqh%2BtdoEimh70vj4Gs5edUaVwjkLEDk2nfOHL5z1cSLR1YScNumlb7qbNpmt2xe53y8BC5iE5Y8uAzf2Dxk6Srq6TZiE5DlQSrruzrwKD00ySYtZ7HlTG9aID2EXZDzSJ%2BFC0ivxo0CkiU9KaEbM2FOo%2F3by%2BTecRBHmvxC3kJiyZnn8fdk5W7d0ouYcjP3b5HBWIhZtDzKdsNlAlFpb0pvm1Ju4MJ6Ro8gGOqUBKVEMPDpiRBTvgOz8MTae%2B8xD2h4QB2vY0Y99OBiaIHHCpP4FTWc88m9vuwE6lM%2FYmbYRa1%2B5vYJYPJ%2FlW%2B92asIZ1egik6%2FWkEUxFIt%2B%2F4Qp9%2FmGR0yz13LJeFaWVxh5J6RLggFZZTu30L%2Bu8ARy2uL9MWeO0XQ6SdosQa0ufKR6eLVCoGjZtiatHKUHukSoW5gXBGYT4UDfRzaPntesDB7GwuhV&X-Amz-Signature=e7ab83464d61cbdca8e8571e72a768472d3f078e4cf0c175ac6b2fc8fbde45a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

