---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLPWQ2X%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlkcWJ9cFD3smsTVwZMvIRFs7oJKJ55IbEYYf5ikP5jAiEA77nMYobYPDsan7u1aj8t%2FVDrFNLoctmVgA4HWjwfkZsq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDARZNjKMKSEYmUvEZCrcA4Rl5Z4bI0v2iTJqc2xjD%2FPCVO%2F%2Fctbv%2Bi7V3jk8%2FeHbcQ2KV6uSYsZwsxxU2nUV0KxEMxMq6zw4ftIsC7b%2BMmrYxzmgeoAGIscSSMdbJ55H2JhQyaIVkLzTCYbW%2F%2F3QXa90ooE6l3XqN1QEwSxECtoM7E%2BmGCKEH1is1Aq42Z1YGqNC%2FY3U%2BznrqEPT1b1k%2BuOFqErlSc7Efbj3Tf10f%2BHRIorM0FwD6XiAn2VhscXJfcYF7ZabykQ0ifJ0OpEmLu89gc0UiGm5%2BIH%2BDaCC0GuUuhZHS1zAwh1EgJyboPd2TK5F2P2gLPQmdtecP8ESPudnM%2FpFWyLGd%2BRNlR8cQ6RstST%2BZTk%2BpajPEQ4w7%2F%2B5Mn9FTt04dV6jKV2fBpFvSO28XmJJ9HjlwmqZqMCHKVcUrG09dBbiB7DS%2F%2FF0O%2BtWDMQDjIT9MnwV1DlZ0xO3XcA0gDNFZfF2RkBtgnW0m3eru00LUvChy3x%2FZMpLbi7qWs3yU%2FOzEGkVlIWZOHxnmENYKoG4h5xcDRXPBOULl8mI60uJvSNXNJi20R%2F8QNOcLUvYtACVj1uD5Y%2F9k3EQTRUzrKL3PJ2e8REHsFYVYxwOAyYadyGKN4Un%2FhXbv87NJvbP0lPa9tPvOWDKMNupv8cGOqUBfGwekJOB6CbiOPp0pzSDdEHl1VzRHeL4MWtW5xk4m%2B7PmXdchuXT15C%2FLpbp7viQtzDJmHvdQVY1per%2FuVmYJL%2F5oPpJlxx%2B%2BEV8UE2x59ngwkIrsenjgpevhT2hkTqyDNlWrWStSFy4A1vpiCIUkRMmuHbW5UaNE63sOqViziwpfiOviU8xHNbVPvLGG3dF0WB2w90LhtS3%2BIaWDFxU4AVMKOSy&X-Amz-Signature=86be3ccab831f427ed16995d2aa600b6a43caa8fcdb3bb631ae6867c7300c4e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

