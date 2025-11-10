---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHW57GDP%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIDPqijqzeC%2FrBQj8JqqgoIVVBS7j%2FVQJyixBGWmRM90gAiBepO46pROF9RStlvn35dnempjyM4gexXFxvZRVDk3MsCr%2FAwgNEAAaDDYzNzQyMzE4MzgwNSIM1%2BiWHw5mYv4bLxmQKtwD66BFEfysSY12Xby2tuzDDva2pm%2B6BqLusabv05TMs330%2BkwV0FIW5yDsQaR%2BDNcO2lsIk24ilbgNDbON8%2FoQ%2FcDEVFLIMpgbotPAfbSQ3aQMHhvySQ9ksDvg6XgMJw7EnX0AvHxZhLlaIMehMk9lnbzxAI5%2Fc94zkd4tTIvBlgtjm%2BGjq1PLCmjnx80REreTc%2FbS%2BrQm0nHT8Tkojt%2FUfuLqdv4A4CmsyWsUAdk5qRcX8prL1mS58hyMufHh8YFSqeh5ntORSI3MnjvQh6XzaA6cGE3SHhVW98VuDy1pojvd2AdN9UbsqLP2Y7Da8jfpoPFJcCA8w1%2FXuhHe6lX4l9yMEXJFq8R9jictdmDNumsgzQtzMM1Xuxbo3uiCpiIoSa3N1nvUNfbbGCg7UJExkWXbBrB%2FDR%2BA0AEECUifzlriCZ7VjdGFwFdlCnA6raKPtssXP6tOfEVa8lr3igrhfST5pXTxLum1a5c5ztr4rouj96lMbCMtWYBOte5wBClqjpE5vBRIrGTDvic5xGoeHCKrENLQ23n3nptLvT6CHU8FqP3UYhfdkq2HF65MU9VPe0BE4cEDd731TGmiRtPbd0%2FHNhS8MxtrE3zy83zGn6bdf82Ow5024%2BeyUaswk4vJyAY6pgGAjiLDAd%2B%2F9fyz2ki0KZId5ssUwfuWcp97LQ2Bi7GtlU6l7OsW%2F9qv2BoQ2vTyLYIAX3E9glX3FHXOYmnogBDPM0YpnBOcCH%2BoecUJjSlVGbqBPjcPmE9bcXi5BYaA1zQpkC7fg%2FwNBJ9OcRUQ4TCid12BVqOsVMPQTT9Y75pyiRQ%2FvOdqqges8MocInmw2N7BYvwfCBf5pxhTp6A3FMctiLVNjb8E&X-Amz-Signature=e6c9816e0f9d738a54e9f8f320760315bb68e2dee111799f9f5b0a2ca52b5685&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

