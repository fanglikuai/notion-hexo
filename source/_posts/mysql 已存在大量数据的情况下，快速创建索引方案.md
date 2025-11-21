---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667B54U6GN%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEwZdC1d8oSdDak9ZaZ0LgHndlqY4p%2BFJPlTHIX0QtlJAiEAxRUGZLUapYhxsUrQaKKsT8ACgmK5eOIYHC26eQBhPRkq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDDyxxu5Kf5lJ0TDuhCrcA%2F3fEISQ%2BRPQCyD0i2Z2akcyUncr2Vr5Kz3yWhYHMqYKxNVWmYkEbnR61uPu7myatfi6sTzJBASkzDv8ZcovNPmDsuX9mD2FUvwSrMi8NL2CMy1YqzMFbfwP2LKTyJQiDQ9qqNfR68Xpu4PwnsEKiqMYwCoSZDIpwgNWiPu4aqOIvJsCLuIXwPzk1VWNWGOBMRVxg9nDwTdaXDFIRTX2pnzGvX0floKa9o0qsckOI8qNjh0G9YwqwfDUqrDkvLj55M1WguKKtQ5MXe5flsERmFpc5aNJN50RKwJhpEiJLX4ymcwFf2uzy1XdzbOSF4i61zcF2FZCKnGmag8YXMTDZw%2BdixXQk%2B%2BQx9DC7TC8h31bKgSw1FWyj44OmMhQAMeGVR%2BWIoTDAx%2BOWMfy1AbzD9AqwvJvAf5Wpyl5YsypNN8CZmW0za8TIi%2FQANIxs6QnIjZrqYM3qh4xoY8bD3e9YfAFseodE05PvGUUAScOra6iSC%2Fc%2F9uxg4G%2BLArv7TraTsqSjcBv9%2BHHHN64RNgo9sUl4TL4S%2BFRdhV7sUpMmY%2F2bMNhWkF2iuTvEgm50oTBXnGBJbE3lNmNZNUQN9ZVOnG10KoA%2Bjoi7%2BCgq18c%2BZVOXQG2ZoHU0uwfoJTDMMPp%2FsgGOqUBiPBV8F0NboaX3SnDcJnkvkQn0YpxHy9f9k9f3cjLsKEubKhVwhdaE4hJNknH7vwSSlHUkh4YPH%2Fgq1BeHIbPBYzwsGVSLq3LUd4V78a6FsR0DnpDdWbfjJBRv1fVtCGXGZK0HUa%2BeJhwWP3nH%2BDjYyOrnyyEz7XWqtWSZO11xJwkRj9ZCKml7CDs1rVWpjASui2PWslbXUDF2q6Im11ImDuWiJTK&X-Amz-Signature=b36d10453b676c4f10238cd4cf11d7850890f318b33611eb13cdcec0fdf316fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

