---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UR3NWFRL%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDTpmFLCzB71TFraJH%2Fe8Sh%2BpY9FmeH71Tgqr5LPc1yHAiEA%2BAQI%2FgTCCPocPBVuAbDWqCBT5sGOoHsMlU3Y0I82JT4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDIvb62czUzuZNQ6WVyrcA30OUbv%2BzEW5gIw%2BITikcLUYLAOaj3CUFILcItALLYIdtzH4B1GiMBJkKvUIxsDj8iU73ncjwpQbG9b4xA%2BLhnc9r0FLDGaGD86i9Xs6o4T9CjSkFvraLVAWHxSCHwL9WObEr4JEP3rRz7o0%2BXOoL7zLxC7GH5c6VrL9P%2FQVtumvO1kOrT0OcgcSy0CJwBqyl%2B9mgMetGdGw5iBH3m3ZO16QLLVbFRVvVWJyUbdq57WvrRCUitXGLaJiIC97lzLlCSZqHzGly3mtw1frlfOVbyRRWT5irFqpdVA3gZtuOtFlfgRbeiwb1I1BzF7dKvICT7i17c7hxHb9H5QbjJu90VrVgkb9dpt6S%2Fw0unReZSabMB0DVVxCizyRFbNLPIw9OXjetk99eVj2Krp9Not%2FVyCUn4pGXHu1ZyuuW5mmzvM64eqcTJTTp%2F6aZlmBoFF3HtDkmS%2BvokOf4Tzs43NqK0AJ4d5UMoQflJmIkuqjE9j%2BVge7fmsWnLlRgoHw%2Ffnuj%2BWb6iB0hBcVtkCwdeqkBavxqr8uMkMrPs5g2MriM5kyAYqiSq0lXmDODftnPbhT9hjnpwxTXhEEsvwlwB63gBi5qf0CVGpZl2raOzjRTHq4RGvk9cNDlnCiIDlaMOqDu8cGOqUB6BbkekKcr0Juyuf23Vvs3h13BlVjf%2FzPzI6tmwMVN%2F9ivVpg8VSwWUfNVwh%2Ba1q02n1tK9SoMqpVuovVUR63OhDILHf2SBroo8Wl7SD0RBuATzpgGz1UwicQlkMXaveDpXlkqvRuo5%2FF7t4%2F2J%2FtS0uqUjYPVPp0LszfvtJ91fJqmhqy%2FX8qjuBwLLu%2BtbQu6GYOrQafD9Ug9zzqzqCRnRlbdR27&X-Amz-Signature=e2da80792c342897905f0ea3e9595c06e93a745baaa9f00592540a0abc8ccb14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

