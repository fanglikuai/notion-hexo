---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YOZXYIU7%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIGob7UOxNnKrb4HfHlaSooaYN50QWqg8fHFzpyySRdUTAiEAnvZQxOZnrjWfK%2BMB7m2%2BC173YEk7uHw0M2Jqv26n36Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDAWZObrWXap5AhKnJircA6W1O7DFpEDGDXIvXpdv%2BTd%2FLIU3GAptNmXzQRqzASxRM3smByOMkbZVOJ%2BZdvC0SY7HxY5MTVt6c6JoVLVPxYRpR1nQcY9P8XeZwt8qB5fDZDXrAPDDCMpMQbElL%2BnDmgIuLqFpoRcUf6USymDpzAZUKqMFXhzIRbVFrKdUJhGln%2FZXY0jD%2FyWz5YE6qqWJC6I1s230EW6dzPSNnArkdIVz3IhxS6HneXyw0fqwTJsoF6P%2FUO0qP8smX8HOkvHvY5er2qheLQsa%2FyUdTEq%2FsVUv5xgJ%2B4ReggeKGOenfhkmxjk379QXtaQV1UJCltXyOw%2FxKOl2%2FP%2Bnw3rDFAAp62%2BNwqLr0DiPNdjBZ%2BBbYsv82RQRIyaAOUMEErId9BzLqyswuTVbVKNPB1PbQH%2Bfr0i76BdIJdNBHcSqi5gO5%2BoVkVaM773EGVgbD9Gnz%2FifuBPGgaKAr3Azx5lGsv2QTKzWkiPXCU8QrVLiMZHjII%2BygTfsgkad5Sh%2BIDsrJY%2FhfO7SFO9l5pZramtow5GvGXI0GuXtUIDPrZmRkByGKflg4ZddO1xnitHwEQCCbvGqRa1WVQxickHQRvcCJ4V%2BkcbDgdJnVmvS8c1jI4vs2wKW2zfHV%2F4xi7BeBZDdMOqLlsgGOqUBXvu6dVcR%2FTU2CSq%2Bsj3itPSS85bjnaW80hMRbwH%2BUuHWWU3MB7P%2FuEcMVUbrmvKesjcQxrMc3qP7uk7J43%2B5%2BBqJSMm520xh64qfhKbck1aG6C4n%2BCFTlSyguGeNrif1vNoma3u%2Ff8IHGMRCahDJrT6l9cO2UGWK2in5QR4qyUMEkijzt3tStiNQOvd4A69YGL6VxQsyslbupbkg35YAcUmXK1DN&X-Amz-Signature=0ad832dafd1328563876af55e3f98542408a51fbeecbf504a37baf7410930543&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

