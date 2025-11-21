---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662IMYL2T%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCudVeQMEXBTwnivbZVonRKs5YO2EfqAlaI4cf9ATJPUwIhAMi8%2Fdt3eO6%2Bqy0VyDQnm39cIX57n7qRR0X%2FsdVwHKPBKv8DCBIQABoMNjM3NDIzMTgzODA1IgxmlBs8rieNMFcSVmUq3AO8dXbmc220AFgTUmdc5GykY1MtCrjo1p0Q0FEtOnJ22HkXiiBt9MQGrV%2FFt9%2BHrRCuVEaUw7af0tYOrgNlOsT%2Famc4fe8pq6%2FKr0n2yA0kJAoYN7%2BMiNFbd99umLag9bIkbYc%2FxHBNqSm%2FxjVf846s8JsX4j%2BRaozBK%2BWgsxl2gViMNVNGlKqnnsCPSM9FlwVXzejYe%2BnrlYBzWJB%2FY8jkyyUOOKpgUtw8Z9T%2BMs2Mkj8F7f%2FRUtyK52TKTGhgTwWEzr427CnZsgs6k6jFODHjMMmPpKQdJYJA%2BmCiS%2B7mvPXpe9epLrR2ihIMMzimBA9eJZA8eWInUfpUen9tgwimw2jFbwllXj9HYFpeKaXCGamCvDL8nZTYhfAY5%2FUcow7hXukpo93SIxMvosjPLwCU9UN35a8qthyQKKnwuTeJepHWJgyeqgI23OagXFj3qkEpaydRj%2BaahE7J8N%2FZpF7w95WJ3Gm0HapvmAtzjtYqBN6i12WhpttpnXuDzED%2Ft8FtYuDUo5hY%2BKjcmS%2FGs%2BjJX%2FpC6Q%2Fu3xOW%2Fy0pIFcG9yoQCMXC5GaIzRCzzK6td9d1NhKUAIszEpmEMP3xNnWq8yv1npcZXAfnpfqqrGGPF5qJxZkfbh%2BWDo8WGTCtrILJBjqkAeNPIBb1tGKZD8H23%2B%2BQtcRGAibBSnMVaQXjbmHnFQ2KJRpxtktpXSSm2M7ewf7oXzOFphGtmhd6dtng%2BOQakVI074WIyJDwCPHVh3bgeAOfm8TDsbV2j36UiSezqGmYPq%2FNd0bZFSo0QnBw5xNv9qT6KyVprhIgi53d01ko%2FuZf9UEpGyfOF3MdJ%2Fp0mraUPSopZ7OYIwfI%2FlZeR2tG%2FwXWXyJ0&X-Amz-Signature=3b4c46f690026e01967415028e51aab5204e7b3ce316fac179ac2a284ed83c29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

