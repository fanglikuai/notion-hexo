---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OHAF2LV%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEst0%2BtQM%2BS3fT8zJu26zsHb90EhZdnZXVpTGuMoUgSaAiBjret5OqdaASgqC1siMhh8uDxbJCEhSBiK9Ia8wm0sayr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMU4w3DjOsj5wQEMHyKtwDSDAOkQLircbIS38fG8TdHJEXMuZp1d4Y%2FIJTAqTZogjs5bv3Jyji5jJxg0NHr%2BZkWGMHQvJbneCE6wufqBHlxgiL%2F8EV5J3qL8PI4jn21qjrxhv86RkSLZYUXiRocWPDAON%2BPOqKkna7IkGW%2BLLuxl7qJceyrBOvMiR25JMibWhTOEeYpR39lEJsBTR5v7P%2BvFY1dRU1dCD%2F%2Bnt7sseb7mqzfyZlOxVP0zhFxYBJxeqvFi4dGFeDKoeMiv8UegTpa3druo6BDAtRrOawagsoq3NCZ5FBQJ72T48GLmHJ4dMIIoemyg2nnlR6%2BpOJGRK0%2Fic19H4%2BzfrjM2wNsVrddRzOYLsxtUdYdPPfxEO7agX%2Frv5qJHh%2BzdhWeoX4%2FcmbfwKB2tzE4WOE5R45YbA4WiplaLLGv44%2FARUwhVcaludieINXYD%2BhtHJhwA4LsMDjrBgamWc7iqdX0CCnqsJoXlUnhJTgHn7xQG34BfeXEMHFcXmrOfFFciGa00lIwc97QkeoF9QHN5dq0XXEQ4702m8fNjPdnr%2B5rxktbpVeA6ErRdGLk789Qsumj%2FeXAzaZBIjs5AjylZyeBQv4p2or7hgElvoaVpcH9O1c9pgXICSsuKQohsxRl9eZiuEwnszfyAY6pgEVfgGN6mbrK473ZUQZ7BL6%2Bnc9TAbX%2BDzw3jWkIuzK36w3vpp9A6LEKFHwjZnfMkxvbeL%2FllKELccPTt8vCS5PFK8z0HEOa%2F3TbQ7UJiaU3rajOIaYuvoDZQ5X1QhEw%2BtACX3vWp0ZDS2%2FDz0eOYpYvb4LmaY47Ko8MkTLNBF%2B7i8%2BZOthRUefXZoccYn4fftsH2m%2BV67%2Fl0GbeRAzLfZjRhdcliTJ&X-Amz-Signature=e59f82070ee9cf89b42019017f60d2872a860347ce2ddc5f220f71bf8dd9071e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

